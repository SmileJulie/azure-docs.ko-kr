---
title: 포함 파일
description: 포함 파일
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 8785f89a335f0ae7d983f267176da1656aee57a0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67182809"
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>사용자 로그인에 MSAL(Microsoft 인증 라이브러리) 사용

1. `index.html` 파일의 `<script></script>` 태그 내에 다음 코드를 추가합니다.

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "Enter_the_Application_Id_here"
            authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };

    var graphConfig = {
        graphMeEndpoint: "https://graph.microsoft.com/v1.0/me"
    };

    // this can be used for login or token request, however in more complex situations
    // this can have diverging options
    var requestObj = {
        scopes: ["user.read"]
    };

    var myMSALObj = new Msal.UserAgentApplication(msalConfig);
    // Register Callbacks for redirect flow
    myMSALObj.handleRedirectCallback(authRedirectCallBack);


    function signIn() {

        myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
            //Login Success
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }).catch(function (error) {
            console.log(error);
        });
    }

    function acquireTokenPopupAndCallMSGraph() {
        //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
        myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
        }).catch(function (error) {
            console.log(error);
            // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
            // Call acquireTokenPopup(popup window)
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
                    callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
                }).catch(function (error) {
                    console.log(error);
                });
            }
        });
    }


    function graphAPICallback(data) {
        document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
    }


    function showWelcomeMessage() {
        var divWelcome = document.getElementById('WelcomeMessage');
        divWelcome.innerHTML = 'Welcome ' + myMSALObj.getAccount().userName + "to Microsoft Graph API";
        var loginbutton = document.getElementById('SignIn');
        loginbutton.innerHTML = 'Sign Out';
        loginbutton.setAttribute('onclick', 'signOut();');
    }


    //This function can be removed if you do not need to support IE
    function acquireTokenRedirectAndCallMSGraph() {
         //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
         myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
             callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
         }).catch(function (error) {
             console.log(error);
             // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
             // Call acquireTokenRedirect
             if (requiresInteraction(error.errorCode)) {
                 myMSALObj.acquireTokenRedirect(requestObj);
             }
         });
     }


    function authRedirectCallBack(error, response) {
        if (error) {
            console.log(error);
        }
        else {
            if (response.tokenType === "access_token") {
                callMSGraph(graphConfig.graphEndpoint, response.accessToken, graphAPICallback);
            } else {
                console.log("token type is:" + response.tokenType);
            }
        }
    }

    function requiresInteraction(errorCode) {
        if (!errorCode || !errorCode.length) {
            return false;
        }
        return errorCode === "consent_required" ||
            errorCode === "interaction_required" ||
            errorCode === "login_required";
    }

    // Browser check variables
    var ua = window.navigator.userAgent;
    var msie = ua.indexOf('MSIE ');
    var msie11 = ua.indexOf('Trident/');
    var msedge = ua.indexOf('Edge/');
    var isIE = msie > 0 || msie11 > 0;
    var isEdge = msedge > 0;
    //If you support IE, our recommendation is that you sign-in using Redirect APIs
    //If you as a developer are testing using Edge InPrivate mode, please add "isEdge" to the if check
    // can change this to default an experience outside browser use
    var loginType = isIE ? "REDIRECT" : "POPUP";

    if (loginType === 'POPUP') {
        if (myMSALObj.getAccount()) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }
    }
    else if (loginType === 'REDIRECT') {
        document.getElementById("SignIn").onclick = function () {
            myMSALObj.loginRedirect(requestObj);
        };
        if (myMSALObj.getAccount() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenRedirectAndCallMSGraph();
        }
    } else {
        console.error('Please set a valid login type');
    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>추가 정보

사용자가 **로그인** 단추를 처음으로 클릭하면 `signIn` 메서드는 사용자를 로그인하기 위해 `loginPopup`를 호출합니다. 이 메서드를 호출한 결과 사용자의 자격 증명을 묻고 유효성을 검사하는 *Microsoft ID 플랫폼 엔드포인트*가 있는 팝업 창이 열립니다. 성공적으로 로그인되면 사용자는 다시 원래 *index.html* 페이지로 리디렉션되고 토큰을 받아 `msal.js`에서 처리되며 토큰에 포함된 정보가 캐시됩니다. 이 토큰은 *ID 토큰*이라고 하며 사용자 표시 이름과 같은 사용자에 대한 기본 정보를 포함합니다. 이 토큰에서 제공하는 데이터를 어떤 용도로든 사용할 계획이면 백 엔드 서버에서 이 토큰의 유효성을 검사하여 토큰이 애플리케이션의 유효한 사용자에게 발급되었음을 보장하는지 확인해야 합니다.

이 가이드에서 생성하는 SPA는 `acquireTokenSilent` 및/또는 `acquireTokenPopup`를 호출하여 사용자 프로필 정보에 대해 Microsoft Graph API를 쿼리하는 데 사용하는 *액세스 토큰*을 가져옵니다. ID 토큰의 유효성을 검사하는 예제가 필요한 경우 GitHub에서 [이](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "GitHub active-directory-javascript-singlepageapp-dotnet-webapi-v2 샘플") 애플리케이션을 살펴보세요. 이 샘플은 토큰 유효성 검사를 위해 ASP.NET Web API를 사용합니다.

#### <a name="getting-a-user-token-interactively"></a>대화형으로 사용자 토큰 가져오기

초기 로그인 후, 사용자는 리소스에 액세스하기 위해 토큰을 요청해야 할 때마다 다시 인증하는 것을 원하지 않으므로 토큰을 획득하는 대부분 경우 *acquireTokenSilent*를 사용해야 합니다. 하지만 사용자가 Microsoft ID 플랫폼 엔드포인트와 강제로 상호 작용하도록 해야 하는 다음과 같은 상황이 있습니다.

- 암호가 만료되어 사용자가 자격 증명을 다시 입력해야 할 수 있습니다.
- 애플리케이션이 사용자 동의가 필요한 리소스에 액세스를 요청하고 있습니다.
- 2단계 인증이 필요합니다.

*acquireTokenPopup*을 호출하면 팝업 창이 표시되고(또는 *acquireTokenRedirect*를 호출하면 사용자가 Microsoft ID 플랫폼 엔드포인트로 리디렉션되고), 사용자는 자격 증명을 확인하거나 필요한 리소스에 동의하거나 2단계 인증을 완료하여 상호 작용해야 합니다.

#### <a name="getting-a-user-token-silently"></a>자동으로 사용자 토큰 가져오기

`acquireTokenSilent` 메서드는 사용자 개입 없이 토큰 획득 및 갱신을 자동으로 처리합니다. `loginPopup`(또는 `loginRedirect`)가 처음으로 실행된 후 일반적으로 `acquireTokenSilent` 메서드를 사용하여 후속 호출 시 보호되는 리소스에 액세스하는 데 사용되는 토큰을 가져옵니다. 즉, 토큰을 요청하거나 갱신하기 위한 호출이 자동으로 수행됩니다.
일부 경우에는 `acquireTokenSilent`가 실패할 수 있습니다(예를 들어 사용자 암호가 만료된 경우). 애플리케이션에서는 이러한 예외를 다음 두 가지 방법으로 처리할 수 있습니다.

1. 즉시 `acquireTokenPopup`에 대한 호출을 수행합니다. 그러면 사용자에게 로그인하라는 메시지가 표시됩니다. 이 패턴은 애플리케이션에 사용자가 사용할 수 있는 인증되지 않은 콘텐츠가 없는 온라인 애플리케이션에서 일반적으로 사용됩니다. 이 설정 안내에서 생성하는 예제는 이 패턴을 사용합니다.

2. 또한 애플리케이션에서는 대화형 로그인이 필요하다는 시각적 표시를 사용자에게 보여줍니다. 따라서 사용자가 로그인할 적절한 시간을 선택하거나 이후에 애플리케이션이 `acquireTokenSilent`를 다시 시작할 수 있습니다. 이는 사용자가 중단 없이 애플리케이션의 기능을 사용할 수 있는 경우(예: 애플리케이션에 사용 가능한 인증되지 않은 콘텐츠가 있는 경우) 일반적으로 사용됩니다. 이 경우 사용자는 보호된 리소스에 액세스하기 위해 로그인하거나, 오래된 정보를 새로 고치는 시점을 결정할 수 있습니다.

> [!NOTE]
> 이 빠른 시작에서는 사용된 브라우저가 Internet Explorer인 경우 Internet Explorer 브라우저의 팝업 창 처리와 관련하여 [알려진 이슈](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) 때문에 `loginRedirect` 및 `acquireTokenRedirect` 메서드를 사용합니다.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>방금 가져온 토큰을 사용하여 Microsoft Graph API 호출

`index.html` 파일의 `<script></script>` 태그 내에 다음 코드를 추가합니다.

```javascript
function callMSGraph(theUrl, accessToken, callback) {
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200)
            callback(JSON.parse(this.responseText));
    }
    xmlHttp.open("GET", theUrl, true); // true for asynchronous
    xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
    xmlHttp.send();
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>보호되는 API에 대한 REST 호출에 관한 추가 정보

이 가이드에서 생성한 애플리케이션 예제에서는 `callMSGraph()` 메서드를 사용하여 토큰이 필요한 보호되는 리소스에 대한 HTTP `GET` 요청을 실행한 다음 콘텐츠를 호출자에게 반환합니다. 이 메서드는 *HTTP 인증 헤더*에 획득된 토큰을 추가합니다. 이 가이드에서 생성한 애플리케이션 예제의 경우 리소스는 사용자 프로필 정보를 표시하는 Microsoft Graph API *me* 엔드포인트입니다.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>사용자를 로그아웃하는 메서드 추가

`index.html` 파일의 `<script></script>` 태그 내에 다음 코드를 추가합니다.

```javascript
/**
 * Sign out the user
 */
 function signOut() {
     myMSALObj.logout();
 }
```

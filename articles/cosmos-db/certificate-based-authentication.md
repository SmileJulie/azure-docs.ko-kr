---
title: Azure Cosmos DB를 사용 하 여 azure Active Directory 인증서 기반 인증
description: Azure Cosmos DB에서 액세스 키에 인증서 기반 인증에 대 한 Azure AD id를 구성 하는 방법에 알아봅니다.
author: voellm
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: tvoellm
ms.reviewer: sngun
ms.openlocfilehash: cc39cc09259c1ae681e1fee070777575e2788323
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827839"
---
# <a name="certificate-based-authentication-for-an-azure-ad-identity-to-access-keys-from-an-azure-cosmos-db-account"></a>Azure Cosmos DB 계정에서 액세스 키를 Azure AD id에 대 한 인증서 기반 인증

인증서 기반 인증에 클라이언트 인증서를 사용 하 여 클라이언트 응용을 프로그램이 Azure Active Directory (Azure AD)를 사용 하 여 인증할 수 있습니다. Azure에서 가상 컴퓨터 또는 온-프레미스 컴퓨터는 같은 id를 해야 하는 컴퓨터에서 인증서 기반 인증을 수행할 수 있습니다. 그런 다음 응용 프로그램 키를 응용 프로그램에서 직접 필요 없이 Azure Cosmos DB 키를 읽을 수 있습니다. 이 문서에서는 샘플 Azure AD 응용 프로그램 만들기, 인증서 기반 인증에 대 한 구성, 새 응용 프로그램 id를 사용 하 여 Azure에 로그인 하는 방법을 설명 하 고 Azure Cosmos 계정에서 키를 검색 합니다. 이 문서에서는 Azure PowerShell을 사용 하 여 id를 설정 하 고 제공 된 C# 샘플 앱을 인증 하 고 Azure Cosmos 계정에서 키에 액세스 합니다.  

## <a name="prerequisites"></a>필수 구성 요소

* 설치 합니다 [최신](/powershell/azure/install-az-ps) 의 Azure PowerShell.

* [Azure 구독](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)을 만듭니다.

## <a name="register-an-app-in-azure-ad"></a>Azure AD에 앱 등록

이 단계에서는 Azure AD 계정에서 샘플 웹 응용 프로그램을 등록 합니다. 이 응용 프로그램은 나중에 Azure Cosmos DB 계정에서 키를 읽는 데 사용 됩니다. 다음 단계를 사용 하 여 응용 프로그램을 등록 합니다. 

1. [Azure 포털](https://portal.azure.com/)할 수 있습니다.

1. Azure를 엽니다 **Active Directory** 창에서 선택한 앱 등록 창으로 이동 **새 등록**합니다. 

   ![Active Directory에서 새 응용 프로그램 등록](./media/certificate-based-authentication/new-app-registration.png)

1. 입력 합니다 **응용 프로그램을 등록** 다음 세부 정보를 사용 하 여 양식:  

   * **이름** – 응용 프로그램의 이름을, "sampleApp"와 같은 모든 이름 수 있습니다.
   * **지원 되는 계정 유형** – 선택 **이 조직 디렉터리에만 (기본 디렉터리) 계정** 이 응용 프로그램 액세스를 현재 디렉터리에 리소스를 허용 하도록 합니다. 
   * **URL 리디렉션** – 형식의 응용 프로그램 선택 **웹** 및 응용 프로그램 호스트 되는 URL을 제공, URL이 될 수 있습니다. 예를 들어 URL 제공할 수 있습니다는 테스트와 같은 `https://sampleApp.com` 앱 존재 하지 않는 경우에 괜찮습니다.

   ![샘플 웹 응용 프로그램 등록](./media/certificate-based-authentication/register-sample-web-app.png)

1. 선택 **등록** 폼을 채운 후 합니다.

1. 앱 등록 되 면 기록해 합니다 **Application(client) ID** 및 **개체 ID**, 다음 단계에서 이러한 세부 정보를 사용 합니다. 

   ![응용 프로그램 및 개체 Id를 가져오려면](./media/certificate-based-authentication/get-app-object-ids.png)

## <a name="install-the-azuread-module"></a>Azure Ad 모듈 설치

이 단계에서는 Azure AD PowerShell 모듈을 설치 합니다. 이 모듈은 이전 단계에서 등록 한 응용 프로그램의 ID를 가져오고 해당 응용 프로그램에 자체 서명 된 인증서를 연결 해야 합니다. 

1. 관리자 권한으로 Windows PowerShell ISE를 엽니다. 아직 수행 하지 않은 경우 AZ PowerShell 모듈을 설치 하 고 구독에 연결 합니다. 여러 구독이 있는 경우에 다음 명령에서 표시 된 것 처럼 현재 구독 컨텍스트를 설정할 수 있습니다.

   ```powershell

   Install-Module -Name Az -AllowClobber
   Connect-AzAccount

   Get-AzSubscription
   $context = Get-AzSubscription -SubscriptionId <Your_Subscription_ID>
   Set-AzContext $context 
   ```

1. 가져와 설치 합니다 [AzureAD](/powershell/module/azuread/?view=azureadps-2.0) 모듈

   ```powershell
   Install-Module AzureAD
   Import-Module AzureAD 
   ```

## <a name="sign-into-your-azure-ad"></a>Azure AD에 로그인

응용 프로그램 등록 하 여 Azure AD에 로그인 합니다. Connect-azuread 명령을 사용 하 여 계정에 로그인 팝업 창에서 Azure 계정 자격 증명을 입력 합니다. 

```powershell
Connect-AzureAD 
```

## <a name="create-a-self-signed-certificate"></a>자체 서명된 인증서 만들기

Windows PowerShell ISE의 다른 인스턴스를 열고 인증서와 연결 된 키를 읽고 자체 서명 된 인증서를 만들려면 다음 명령을 실행 합니다.

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "Cert:\CurrentUser\My" -Subject "CN=sampleAppCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData()) 
```

## <a name="create-the-certificate-based-credential"></a>인증서 기반 자격 증명 만들기 

다음 인증서 기반 자격 증명을 만들고 응용 프로그램의 개체 ID를 가져오려면 다음 명령을 실행 합니다. 이 예제에서는 1 년 후에 만료 되도록 인증서 설정, 모든 필수 종료 날짜를 설정할 수 있습니다.

```powershell
$application = Get-AzureADApplication -ObjectId <Object_ID_of_Your_Application>

New-AzureADApplicationKeyCredential -ObjectId $application.ObjectId -CustomKeyIdentifier "Key1" -Type AsymmetricX509Cert -Usage Verify -Value $keyValue -EndDate "2020-01-01"
```

위의 명령은 아래 스크린샷과 유사한 출력이 발생합니다.

![인증서 기반 자격 증명 만들기 결과](./media/certificate-based-authentication/certificate-based-credential-output.png)

## <a name="configure-your-azure-cosmos-account-to-use-the-new-identity"></a>새 id를 사용 하 여 Azure Cosmos 계정 구성

1. [Azure 포털](https://portal.azure.com/)할 수 있습니다.

1. Azure Cosmos 계정의 열기로 이동 합니다 **액세스 제어 (IAM)** 블레이드입니다.

1. 선택 **추가** 하 고 **역할 할당 추가**합니다. 추가 이전 단계에서 만든 sampleApp **참가자** 다음 스크린샷에 표시 된 대로 역할:

   ![새 id를 사용 하려면 Azure Cosmos 계정 구성](./media/certificate-based-authentication/configure-cosmos-account-with-identify.png)

1. 선택 **저장할** 양식을 작성 한 후


## <a name="access-the-keys-from-powershell"></a>PowerShell에서 키에 액세스

이 단계에서는 응용 프로그램 및 인증서 생성 및 액세스 Azure Cosmos 계정 키를 사용 하 여 Azure에 로그인 하는 것이 됩니다. 

1. 처음에 계정에 로그인 하는를 사용 하는 Azure 계정의 자격 증명을 선택을 취소 합니다. 다음 명령을 사용 하 여 자격 증명을 지울 수 있습니다.

   ```powershell
   Disconnect-AzAccount -Username <Your_Azure_account_email_id> 
   ```

1. 다음 응용 프로그램의 자격 증명을 사용 하 여 Azure portal에 로그인 하 고 Azure Cosmos DB 키에 액세스할 수 있는지 확인 합니다.

   ```powershell
   Login-AzAccount -ApplicationId <Your_Application_ID> -CertificateThumbprint $cert.Thumbprint -ServicePrincipal -Tenant <Tenant_ID_of_your_application>

   Invoke-AzResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDB/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <Resource_Group_Name_of_your_Azure_Cosmos_account> -ResourceName <Your_Azure_Cosmos_Account_Name> 
   ```

이전 명령에는 Azure Cosmos 계정의 기본 및 보조 마스터 키 표시 됩니다. Get 키 요청에 성공 하 고 "sampleApp" 응용 프로그램에서 시작 되는 이벤트의 유효성을 검사 하려면 Azure Cosmos 계정의 활동 로그를 볼 수 있습니다. 
 
![Azure AD에서 get 키 호출의 유효성을 검사합니다](./media/certificate-based-authentication/activity-log-validate-results.png)


## <a name="access-the-keys-from-a-c-application"></a>액세스에서 키를 C# 응용 프로그램 

이 시나리오에서 키에 액세스 하 여 검증할 수도 있습니다는 C# 응용 프로그램입니다. 다음 C# 콘솔 응용 프로그램, Active Directory에 등록 된 앱을 사용 하 여 Azure Cosmos DB 키에 액세스할 수 있는 합니다. TenantId, clientID, certName, 리소스 그룹 이름, 구독 ID 업데이트 하도록 코드를 실행 하기 전에 Azure Cosmos 계정 이름 세부 정보입니다. 

```csharp
using System;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Linq;
using System.Net.Http;
using System.Security.Cryptography.X509Certificates;
using System.Threading;
using System.Threading.Tasks;
 
namespace TodoListDaemonWithCert
{
    class Program
    {
        private static string aadInstance = "https://login.windows.net/";
        private static string tenantId = "<Your_Tenant_ID>";
        private static string clientId = "<Your_Client_ID>";
        private static string certName = "<Your_Certificate_Name>";
 
        private static int errorCode = 0;
        static int Main(string[] args)
        {
            MainAync().Wait();
            Console.ReadKey();
 
            return 0;
        } 
 
        static async Task MainAync()
        {
            string authContextURL = aadInstance + tenantId;
            AuthenticationContext authContext = new AuthenticationContext(authContextURL);
            X509Certificate2 cert = ReadCertificateFromStore(certName);
 
            ClientAssertionCertificate credential = new ClientAssertionCertificate(clientId, cert);
            AuthenticationResult result = await authContext.AcquireTokenAsync("https://management.azure.com/", credential);
            if (result == null)
            {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }
 
            string token = result.AccessToken;
            string subscriptionId = "<Your_Subscription_ID>";
            string rgName = "<ResourceGroup_of_your_Cosmos_account>";
            string accountName = "<Your_Cosmos_account_name>";
            string cosmosDBRestCall = $"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{rgName}/providers/Microsoft.DocumentDB/databaseAccounts/{accountName}/listKeys?api-version=2015-04-08";
 
            Uri restCall = new Uri(cosmosDBRestCall);
            HttpClient httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Remove("Authorization");
            httpClient.DefaultRequestHeaders.Add("Authorization", "Bearer " + token);
            HttpResponseMessage response = await httpClient.PostAsync(restCall, null);
 
            Console.WriteLine("Got result {0} and keys {1}", response.StatusCode.ToString(), response.Content.ReadAsStringAsync().Result);
        }
 
 
        /// <summary>
        /// Reads the certificate
        /// </summary>
        private static X509Certificate2 ReadCertificateFromStore(string certName)
        {
            X509Certificate2 cert = null;
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection certCollection = store.Certificates;
 
            // Find unexpired certificates.
            X509Certificate2Collection currentCerts = certCollection.Find(X509FindType.FindByTimeValid, DateTime.Now, false);
 
            // From the collection of unexpired certificates, find the ones with the correct name.
            X509Certificate2Collection signingCert = currentCerts.Find(X509FindType.FindBySubjectName, certName, false);
 
            // Return the first certificate in the collection, has the right name and is current.
            cert = signingCert.OfType<X509Certificate2>().OrderByDescending(c => c.NotBefore).FirstOrDefault();
            store.Close();
            return cert;
        }
 
 
        /// <summary>
        /// Get an access token from Azure AD using client credentials.
        /// If the attempt to get a token fails because the server is unavailable, retry twice after 3 seconds each
        /// </summary>
        private static async Task<AuthenticationResult> GetAccessToken(AuthenticationContext authContext, string resourceUri, ClientAssertionCertificate cert)
        {
            //
            // Get an access token from Azure AD using client credentials.
            // If the attempt to get a token fails because the server is unavailable, retry twice after 3 seconds each.
            //
            AuthenticationResult result = null;
            int retryCount = 0;
            bool retry = false;
 
            do
            {
                retry = false;
                errorCode = 0;
 
                try
                {
                    result = await authContext.AcquireTokenAsync(resourceUri, cert);
                }
                catch (AdalException ex)
                {
                    if (ex.ErrorCode == "temporarily_unavailable")
                    {
                        retry = true;
                        retryCount++;
                        Thread.Sleep(3000);
                    }
 
                    Console.WriteLine(
                        String.Format("An error occurred while acquiring a token\nTime: {0}\nError: {1}\nRetry: {2}\n",
                        DateTime.Now.ToString(),
                        ex.ToString(),
                        retry.ToString()));
 
                    errorCode = -1;
                }
 
            } while ((retry == true) && (retryCount < 3));
            return result;
        }
    }
}
```

이 스크립트는 다음 스크린샷에 표시 된 대로 기본 및 보조 마스터 키를 출력 합니다.

![csharp 응용 프로그램 출력](./media/certificate-based-authentication/csharp-application-output.png)

이전 섹션에는 마찬가지로 get 키 요청 이벤트 "sampleApp" 응용 프로그램에서 시작 되도록 유효성을 검사 하 여 Azure Cosmos 계정의 활동 로그를 볼 수 있습니다. 


## <a name="next-steps"></a>다음 단계

* [Azure Key Vault를 사용 하 여 Azure Cosmos 키 보호](access-secrets-from-keyvault.md)

* [Azure Cosmos DB의 보안 특성](cosmos-db-security-attributes.md)

---
title: Azure App Service Linux Java 앱 구성 | Microsoft Docs
description: Linux의 Azure App Service에서 실행되는 Java 앱을 구성하는 방법을 알아봅니다.
keywords: azure 앱 서비스, 웹 앱, linux, oss, java, java ee을 jee, javaee
services: app-service
author: bmitchell287
manager: douge
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/26/2019
ms.author: brendm
ms.custom: seodec18
ms.openlocfilehash: af6fd7b99147396a70fccc7b2b11dfef3def15a8
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786303"
---
# <a name="configure-a-linux-java-app-for-azure-app-service"></a>Azure App Service에 대 한 Linux Java 앱 구성

Linux 기반의 Azure App Service는 Java 개발자가 Tomcat 또는 Java SE(Standard Edition) 패키지 웹 애플리케이션을 신속하게 구축하고, 완벽하게 관리되는 Linux 기반 서비스에 배포하고, 규모를 조정할 수 있게 도와줍니다. 명령줄에서 또는 IntelliJ, Eclipse, Visual Studio Code 같은 편집기에서 Maven 플러그 인을 사용하여 애플리케이션을 배포할 수 있습니다.

이 가이드는 주요 개념 및 기본 제공 Linux 컨테이너를 사용 하 여 App Service에서 Java 개발자를 위한 지침을 제공 합니다. Azure App Service를 사용한 경험이 없는 경우에 따라 합니다 [Java 빠른 시작](quickstart-java.md) 하 고 [PostgreSQL 자습서를 사용 하 여 Java](tutorial-java-enterprise-postgresql-app.md) 첫 번째입니다.

## <a name="deploying-your-app"></a>앱 배포

사용할 수 있습니다 [Azure App Service의 Maven 플러그 인](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) .jar와.war 파일을 배포 합니다. 사용 하 여 인기 있는 Ide 사용 하 여 배포도 지원 됩니다 [IntelliJ 용 Azure 도구 키트](/java/azure/intellij/azure-toolkit-for-intellij) 하거나 [Eclipse 용 Azure 도구 키트](/java/azure/eclipse/azure-toolkit-for-eclipse)합니다.

그렇지 않으면 배포 방법 보관 형식에 따라 달라 집니다.

- .war 파일을 Tomcat에 배포하려면 `/api/wardeploy/` 엔드포인트를 사용하여 보관 파일을 게시합니다. 이 API에 대한 자세한 내용은 [이 설명서](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file)를 참조하세요.
- Java SE 이미지에서 .jar 파일을 배포하려면 Kudu 사이트의 `/api/zipdeploy/` 엔드포인트를 사용합니다. 이 API에 대한 자세한 내용은 [이 설명서](https://docs.microsoft.com/azure/app-service/deploy-zip#rest)를 참조하세요.

FTP를 사용하여 .war 또는.jar을 배포하지 마십시오. FTP 도구는 시작 스크립트, 종속성 또는 기타 런타임 파일을 업로드하기 위해 설계되었습니다. 웹앱을 배포하기 위한 최적의 선택은 아닙니다.

## <a name="logging-and-debugging-apps"></a>앱 로깅 및 디버깅

Azure Portal을 통해 각 앱에 대한 성능 보고서, 트래픽 시각화 및 상태 확인을 사용할 수 있습니다. 자세한 내용은 [Azure App Service 진단 개요](../overview-diagnostics.md)합니다.

### <a name="ssh-console-access"></a>SSH 콘솔 액세스

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>진단 로그 스트림

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

자세한 내용은 [Azure CLI를 사용하여 로그 스트리밍](../troubleshoot-diagnostic-logs.md#streaming-with-azure-cli)을 참조하세요.

### <a name="app-logging"></a>앱 로깅

Azure Portal 또는 [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config)를 통해 [애플리케이션 로깅](../troubleshoot-diagnostic-logs.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#enablediag)을 사용하도록 설정하여 애플리케이션의 표준 콘솔 출력 및 표준 콘솔 오류 스트림을 로컬 파일 시스템 또는 Azure Blob Storage에 쓰도록 App Service를 구성할 수 있습니다. 로컬 App Service 파일 시스템 인스턴스에 로깅하는 동작은 구성된 지 12시간 후에 비활성화 됩니다. 더 긴 시간 동안 보존하기를 원하는 경우 Blob Storage 컨테이너에 출력을 쓰도록 응용 프로그램을 구성합니다. Java 및 Tomcat 응용 프로그램 로그에서 찾을 수 있습니다 합니다 */home/LogFiles/응용프로그램/* 디렉터리입니다.

애플리케이션에서 [Logback](https://logback.qos.ch/) 또는 [Log4j](https://logging.apache.org/log4j)를 추적에 사용하는 경우 [Application Insights에서 Java 추적 로그 탐색](/azure/application-insights/app-insights-java-trace-logs)의 로깅 프레임워크 구성 지침에 따라 이러한 추적 로그를 Azure Application Insights로 전송하여 검토할 수 있습니다.

### <a name="troubleshooting-tools"></a>문제 해결 도구

기본 제공 Java 이미지를 기반으로 합니다 [Alpine Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html) 운영 체제입니다. 사용 된 `apk` 문제 해결을 설치 하려면 패키지 관리자 도구 또는 명령입니다.

### <a name="flight-recorder"></a>비행 레코더

App Service의 모든 Linux Java 이미지에는 쉽게 JVM에 연결 하 고 기록 하는 프로파일러를 시작 하거나 생성할 수 있습니다 힙 덤프를 설치 하는 Zulu 비행 레코더는 있습니다.

#### <a name="timed-recording"></a>시간 초과 기록

시작, App Service에 대 한 SSH 가져오고 실행 하는 `jcmd` 실행 중인 모든 Java 프로세스의 목록을 보려면 명령입니다. 프로세스 ID 번호 (pid)를 사용 하 여 실행 중인 Java 응용 프로그램 자체 jcmd, 외에도 표시 됩니다.

```shell
078990bbcd11:/home# jcmd
Picked up JAVA_TOOL_OPTIONS: -Djava.net.preferIPv4Stack=true
147 sun.tools.jcmd.JCmd
116 /home/site/wwwroot/app.jar
```

30 초 기록 하면 JVM 시작 하려면 아래 명령을 실행 합니다. JVM을 프로 파일링 되 고 JFR 파일을 만듭니다 *jfr_example.jfr* 홈 디렉터리에 있습니다. (Java 앱의 pid를 사용 하 여 116을 바꿉니다.)

```shell
jcmd 116 JFR.start name=MyRecording settings=profile duration=30s filename="/home/jfr_example.jfr"
```

녹음/녹화를 확인할 수는 30 초 간격 동안 수행 되 고 실행 하 여 `jcmd 116 JFR.check`. 지정된 된 Java 프로세스에 대 한 모든 기록 표시 됩니다.

#### <a name="continuous-recording"></a>연속 기록

지속적으로 런타임 성능에 영향을 최소화 하면서 Java 응용 프로그램 프로 파일링 하려면 Zulu 비행 레코더를 사용할 수 있습니다 ([원본](https://assets.azul.com/files/Zulu-Mission-Control-data-sheet-31-Mar-19.pdf)). 이렇게 하려면 필요한 구성을 사용 하 여 JAVA_OPTS 라는 앱 설정을 만들고 다음 Azure CLI 명령을 실행 합니다. JAVA_OPTS 앱 설정의 내용을 전달 되는 `java` 앱이 시작 될 때 명령입니다.

```azurecli
az webapp config appsettings set -g <your_resource_group> -n <your_app_name> --settings JAVA_OPTS=-XX:StartFlightRecording=disk=true,name=continuous_recording,dumponexit=true,maxsize=1024m,maxage=1d
```

자세한 내용은 참조 하십시오 합니다 [Jcmd 명령 참조](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190)합니다.

### <a name="analyzing-recordings"></a>녹음/녹화를 분석합니다.

사용 하 여 [FTPS](../deploy-ftp.md) JFR 파일에 로컬 컴퓨터에 다운로드 합니다. 파일을 분석 JFR, 다운로드 및 설치 [Zulu 관제](https://www.azul.com/products/zulu-mission-control/)합니다. 를 Zulu 임무 컨트롤에 대 한 참조를 [Azul 설명서](https://docs.azul.com/zmc/) 하며 [설치 지침](https://docs.microsoft.com/java/azure/jdk/java-jdk-flight-recorder-and-mission-control).

## <a name="customization-and-tuning"></a>사용자 지정 및 튜닝

Linux 용 azure App Service에서 Azure portal 및 CLI를 통해 사용자 지정 하 고 상자 튜닝을 지원합니다. 비 특정 Java 웹 앱 구성에 대 한 다음 문서를 검토 합니다.

- [앱 설정 구성](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)
- [사용자 지정 도메인 설정](../app-service-web-tutorial-custom-domain.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [SSL 사용](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [CDN 추가](../../cdn/cdn-add-to-web-app.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Kudu 사이트를 구성 합니다.](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Java 런타임 옵션 설정

Tomcat 및 Java SE 환경에서 할당 된 메모리 또는 기타 JVM 런타임 옵션을 설정 하려면 만듭니다는 [앱 설정](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) 라는 `JAVA_OPTS` 옵션을 사용 합니다. App Service Linux는 시작될 때 이 설정을 Java 런타임에 환경 변수로 전달합니다.

Azure Portal에서, 웹앱의 **애플리케이션 설정** 아래에서 `-Xms512m -Xmx1204m`처럼 추가 설정을 포함하는 `JAVA_OPTS`라고 하는 새 앱 설정을 만듭니다.

Maven 플러그 인에서 앱 설정을 구성 하려면 Azure 플러그 인 섹션의 설정/값 태그를 추가 합니다. 다음 예제에서는 특정 최소 및 최대 Java 힙 크기를 설정합니다.

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

App Service 계획에서 배포 슬롯 하나를 사용하여 단일 애플리케이션을 실행하는 개발자는 다음 옵션을 사용할 수 있습니다.

- B1 및 S1의 경우 인스턴스: `-Xms1024m -Xmx1024m`
- B2 및 s2의 경우 인스턴스: `-Xms3072m -Xmx3072m`
- B3 및 S3 인스턴스: `-Xms6144m -Xmx6144m`

애플리케이션 힙 설정을 튜닝할 때 App Service 계획 세부 정보를 검토하고 여러 애플리케이션 및 배포 슬롯 요구 사항을 고려하여 최적의 메모리 할당을 찾아보세요.

JAR 응용 프로그램을 배포 하는 경우 그 이름은 *app.jar* 기본 제공 이미지를 앱에 올바르게 식별할 수 있도록 합니다. (Maven 플러그 인은 자동으로 변경 합니다.) 경우 원하지 않는에 JAR을 바꾸려면 *app.jar*, JAR를 실행 하려면 명령 사용 하 여 셸 스크립트를 업로드할 수 있습니다. 이 스크립트에 전체 경로 붙여 합니다 [시작 파일](app-service-linux-faq.md#built-in-images) 포털의 구성 섹션에서 텍스트 상자에 붙여넣습니다.

### <a name="turn-on-web-sockets"></a>웹 소켓 켜기

애플리케이션의 Azure Portal **애플리케이션 설정**에서 웹 소켓을 켭니다. 설정을 적용하려면 애플리케이션을 다시 시작해야 합니다.

Azure CLI에서 다음 명령을 사용하여 웹 소켓 지원을 켭니다.

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

그런 다음, 애플리케이션을 다시 시작합니다.

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>기본 문자 인코딩 설정

Azure Portal에서, 웹앱의 **애플리케이션 설정** 아래에 `-Dfile.encoding=UTF-8` 값을 사용하여 `JAVA_OPTS`이라고 하는 새 앱 설정을 만듭니다.

또는 App Service Maven 플러그 인을 사용하여 앱 설정을 구성할 수 있습니다. 플러그 인 구성에서 설정 이름 및 값 태그를 추가합니다.

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="adjust-startup-timeout"></a>시작 시간 제한을 조정합니다

Java 응용 프로그램 특히 큰 경우에 시작 시간 제한을 늘려야 합니다. 이렇게 하려면 응용 프로그램 설정 만들기 `WEBSITES_CONTAINER_START_TIME_LIMIT` 앱 서비스 시간 초과 전까지 대기 해야 하는 시간 (초) 수로 설정 합니다. 최대값은 `1800` 시간 (초)입니다.

### <a name="pre-compile-jsp-files"></a>JSP 파일을 미리 컴파일

Tomcat 응용 프로그램의 성능 향상을 위해 App Service에 배포 하기 전에 JSP 파일을 컴파일할 수 있습니다. 사용할 수는 [Maven 플러그 인](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) Apache 선회 비행 1, 또는이 사용 하 여 제공한 [Ant 빌드 파일](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation)합니다.

## <a name="secure-applications"></a>보안 애플리케이션

Linux용 App Service에서 실행되는 Java 애플리케이션의 [보안 모범 사례](/azure/security/security-paas-applications-using-app-services) 집합은 다른 애플리케이션과 동일합니다.

### <a name="authenticate-users"></a>사용자 인증

사용 하 여 Azure portal에서 앱 인증을 설정 합니다 **인증 및 권한 부여** 옵션입니다. 여기서 Azure Active Directory 또는 Facebook, Google, GitHub 등의 소셜 로그인을 사용하여 인증을 사용하도록 설정할 수 있습니다. Azure Portal 구성은 단일 인증 공급자를 구성할 때만 작동합니다. 자세한 내용은 [Azure Active Directory 로그인을 사용하도록 App Service 앱 구성](../configure-authentication-provider-aad.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) 및 기타 ID 공급자 관련 문서를 참조하세요. 여러 로그인 공급자를 사용하도록 설정해야 하는 경우 [App Service 인증 사용자 지정](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) 문서의 지침을 따르세요.

#### <a name="tomcat"></a>Tomcat

사용자의 Tomcat 응용 프로그램에 액세스할 수 클레임 직접 보안 주체를 캐스팅 하 여 Tomcat servlet에서 개체에 Map 개체입니다. Map 개체에는 해당 형식에 대 한 클레임의 컬렉션에 각 클레임 유형에 매핑됩니다. 아래 코드에 `request` 의 인스턴스가 `HttpServletRequest`합니다.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

검사할 수 이제는 `Map` 특정 클레임에 대 한 개체입니다. 예를 들어, 다음 코드 조각은 모든 클레임 유형이을 반복 하며 각 컬렉션의 내용을 인쇄 합니다.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

사용자 로그 아웃 하 고 다른 작업을 수행 하려면 설명서를 참조 하십시오 온 [App Service 인증 및 권한 부여 사용](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to)합니다. Tomcat의 공식적인 설명서 이기도 [HttpServletRequest 인터페이스](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) 및 해당 메서드. 메서드는 또한 하이드레이션 다음 servlet App Service 구성에 따라:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

이 기능을 사용 하지 않으려면 라는 응용 프로그램 설정을 만들고 `WEBSITE_AUTH_SKIP_PRINCIPAL` 값을 사용 하 여 `1`입니다. App Service에서 추가 하는 모든 servlet 필터를 사용 하지 않으려면 이라는 설정을 만들 `WEBSITE_SKIP_FILTERS` 값을 사용 하 여 `1`입니다.

#### <a name="spring-boot"></a>Spring Boot

Spring Boot 개발자는 [Azure Active Directory Spring Boot starter](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable)를 사용하여 친숙한 Spring Security 주석 및 API로 애플리케이션을 보호할 수 있습니다. 최대 헤더 크기를 늘려야 하 *application.properties* 파일입니다. 이 값은 `16384`로 설정하는 것이 좋습니다.

### <a name="configure-tlsssl"></a>TLS/SSL 구성

[기존 사용자 지정 SSL 인증서 바인딩](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)의 지침에 따라 기존 SSL 인증서를 업로드하고 애플리케이션의 도메인 이름에 바인딩합니다. 기본적으로 애플리케이션에서 HTTP 연결을 계속 허용합니다. 자습서의 단계에 따라 SSL 및 TLS를 적용하세요.

### <a name="use-keyvault-references"></a>사용 하 여 KeyVault 참조

[Azure KeyVault](../../key-vault/key-vault-overview.md) 액세스 정책 및 감사 기록 사용 하 여 중앙 집중식된 보안 관리를 제공 합니다. KeyVault에 비밀 (예: 암호 또는 연결 문자열)을 저장 하 고 환경 변수를 통해 응용 프로그램에서 이러한 비밀에 액세스할 수 있습니다.

먼저 지침을 따르세요 [Key Vault에 응용 프로그램 액세스 권한을 부여](../app-service-key-vault-references.md#granting-your-app-access-to-key-vault) 하 고 [비밀에 대 한 KeyVault 참조 응용 프로그램 설정에서 하](../app-service-key-vault-references.md#reference-syntax)합니다. App Service 터미널을 원격으로 액세스 하는 동안 환경 변수를 인쇄 하 여 암호에 대 한 참조 확인을 확인할 수 있습니다.

Spring 또는 Tomcat 구성 파일에서 이러한 암호를 삽입, 환경 변수 주입 구문을 사용 하 여 (`${MY_ENV_VAR}`). Spring 구성 파일에 대 한이 설명서를 참조 하십시오이 온 [구성을 표면화](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)합니다.

## <a name="configure-apm-platforms"></a>APM 플랫폼 구성

이 섹션에서는 NewRelic 및 AppDynamics 응용 프로그램 성능 모니터링 (APM) 플랫폼을 사용 하 여 Linux의 Azure App Service에 배포 된 Java 응용 프로그램을 연결 하는 방법을 보여 줍니다.

[New Relic 구성](#configure-new-relic)
[AppDynamics 구성](#configure-appdynamics)

### <a name="configure-new-relic"></a>New Relic 구성

1. [NewRelic.com](https://newrelic.com/signup)에서 NewRelic 계정 만들기
2. NewRelic에서 Java 에이전트를 다운로드, 유사한 파일 이름을 더 *newrelic-java-x.x.x.zip*합니다.
3. 라이선스 키를 복사합니다. 이 키는 나중에 에이전트를 구성하는 데 필요합니다.
4. [App Service 인스턴스에 대 한 SSH](app-service-linux-ssh-support.md) 새 디렉터리를 만들고 */home/site/wwwroot/apm*합니다.
5. 디렉터리에 압축 푼된 NewRelic Java 에이전트 파일을 업로드 */home/site/wwwroot/apm*합니다. 에이전트에 대해 파일에 있어야 */home/site/wwwroot/apm/newrelic*합니다.
6. YAML 파일을 수정 */home/site/wwwroot/apm/newrelic/newrelic.yml* 고유한 라이선스 키를 사용 하 여 자리 표시자 라이선스 값으로 바꿉니다.
7. Azure Portal의 App Service에서 사용자 애플리케이션을 찾아 새 애플리케이션 설정을 만듭니다.
    - 앱이 **Java SE**를 사용하는 경우 값이 `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`인 `JAVA_OPTS`라는 환경 변수를 만듭니다.
    - **Tomcat**을 사용하는 경우 값이 `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`인 `CATALINA_OPTS`라는 환경 변수를 만듭니다.
    - 사용 중인 경우 **WildFly**, New Relic 설명서를 참조 하십시오 [여기](https://docs.newrelic.com/docs/agents/java-agent/additional-installation/wildfly-version-11-installation-java) JBoss 구성과 Java 에이전트를 설치 하는 방법에 대 한 지침에 대 한 합니다.
    - `JAVA_OPTS` 또는 `CATALINA_OPTS`에 대한 환경 변수가 이미 있는 경우 현재 값의 끝에 `javaagent` 옵션을 추가합니다.

### <a name="configure-appdynamics"></a>AppDynamics 구성

1. [AppDynamics.com](https://www.appdynamics.com/community/register/)에서 AppDynamics 계정 만들기
2. AppDynamics 웹 사이트에서 Java 에이전트를 다운로드, 파일 이름을 유사한 됩니다 *AppServerAgent x.x.x.xxxxx.zip*
3. [App Service 인스턴스에 대 한 SSH](app-service-linux-ssh-support.md) 새 디렉터리를 만들고 */home/site/wwwroot/apm*합니다.
4. Java 에이전트 파일 디렉터리에 업로드 */home/site/wwwroot/apm*합니다. 에이전트에 대해 파일에 있어야 */home/site/wwwroot/apm/appdynamics*합니다.
5. Azure Portal의 App Service에서 사용자 애플리케이션을 찾아 새 애플리케이션 설정을 만듭니다.
    - **Java SE**를 사용하는 경우 값이 `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`인 `JAVA_OPTS`라는 환경 변수를 만듭니다. 여기서 `<app-name>`은 App Service 이름입니다.
    - **Tomcat**을 사용하는 경우 값이 `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`인 `CATALINA_OPTS`라는 환경 변수를 만듭니다. 여기서 `<app-name>`은 App Service 이름입니다.
    - 사용 중인 경우 **WildFly**, AppDynamics 설명서를 참조 하십시오 [여기](https://docs.appdynamics.com/display/PRO45/JBoss+and+Wildfly+Startup+Settings) JBoss 구성과 Java 에이전트를 설치 하는 방법에 대 한 지침에 대 한 합니다.

## <a name="configure-jar-applications"></a>JAR 응용 프로그램 구성

### <a name="starting-jar-apps"></a>JAR 앱 시작

기본적으로 App Service는 이라는 이름으로 JAR 응용 프로그램 *app.jar*합니다. 이 이름에 해당 하는 경우 자동으로 실행 됩니다. Maven 사용자에 게는 JAR 이름을 포함 하 여 설정할 수 있습니다 `<finalName>app</finalName>` 에 `<build>` 섹션에 *pom.xml*합니다. [Gradle에서 동일 하 게 수행할 수 있습니다](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html#org.gradle.api.tasks.bundling.Jar:archiveFileName) 설정 하 여는 `archiveFileName` 속성입니다.

JAR에 대 한 다른 이름을 사용 하려는 경우 제공 해야 합니다 [시작 명령](app-service-linux-faq.md#built-in-images) JAR 파일을 실행 하는 합니다. `java -jar my-jar-app.jar` )을 입력합니다. 구성 포털의 시작 명령에 대 한 값을 설정할 수 있습니다 > 일반 설정 또는 이름이 응용 프로그램 설정을 `STARTUP_COMMAND`합니다.

### <a name="server-port"></a>서버 포트

App Service Linux 응용 프로그램으로 포트 80에서 수신 대기 해야 하므로 포트 80에 들어오는 요청을 라우팅합니다. 응용 프로그램의 구성에서이 수행할 수 있는 (Spring의 같은 *application.properties* 파일), 또는 시작 명령 (예를 들어 `java -jar spring-app.jar --server.port=80`). 일반적인 Java 프레임 워크에 대 한 다음 설명서를 참조 하세요.

- [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-use-short-command-line-arguments)
- [SparkJava](http://sparkjava.com/documentation#embedded-web-server)
- [Micronaut](https://docs.micronaut.io/latest/guide/index.html#runningSpecificPort)
- [Play Framework](https://www.playframework.com/documentation/2.6.x/ConfiguringHttps#Configuring-HTTPS)
- [Vertx](https://vertx.io/docs/vertx-core/java/#_start_the_server_listening)
- [Quarkus](https://quarkus.io/guides/application-configuration-guide)

## <a name="data-sources"></a>데이터 소스

### <a name="tomcat"></a>Tomcat

이러한 지침은 데이터베이스 연결에 적용됩니다. 선택한 데이터베이스의 드라이버 클래스 이름 및 JAR 파일로 자리 표시자를 채워야 합니다. 공통 데이터베이스에 대한 클래스 이름 및 드라이버 다운로드가 포함된 표가 제공됩니다.

| 데이터베이스   | 드라이버 클래스 이름                             | JDBC 드라이버                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [다운로드](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [다운로드](https://dev.mysql.com/downloads/connector/j/)(“플랫폼 독립적” 선택) |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [다운로드](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#available-downloads-of-jdbc-driver-for-sql-server)                                                           |

데이터베이스 연결 JDBC (Java) 또는 Java 지 속성 API (JPA) 사용 하 여 Tomcat을 구성 하려면 먼저 사용자 지정을 `CATALINA_OPTS` 시작 시에 Tomcat 읽을 수 있는 환경 변수입니다. [App Service Maven 플러그 인](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)에서 앱 설정을 통해 이러한 값을 설정합니다.

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

환경 변수를 설정 또는 합니다 **구성** > **응용 프로그램 설정** Azure 포털의 페이지입니다.

다음으로, 데이터 원본을 한 애플리케이션에만 제공할 것인지 또는 Tomcat 서블릿에서 실행 중인 모든 애플리케이션에 제공할 것인지 결정합니다.

#### <a name="application-level-data-sources"></a>응용 프로그램 수준 데이터 원본

1. 만들기는 *context.xml* 파일을 *메타 INF /* 프로젝트의 디렉터리입니다. 만들기는 *META INF /* 디렉터리 존재 하지 않는 경우.

2. *context.xml*에 추가 `Context` 데이터 소스를 JNDI 주소를 연결할 요소입니다. `driverClassName` 자리 표시자를 위 테이블에 있는 드라이버의 클래스 이름으로 바꿉니다.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. 응용 프로그램을 업데이트 *web.xml* 응용 프로그램에서 데이터 소스를 사용할 수 있습니다.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>서버 수준 리소스를 공유합니다.

1. 내용을 복사 합니다 */usr/local/tomcat/conf* 로 */home/tomcat/conf* 아직 구성이 있는 경우 SSH를 사용 하 여 App Service Linux 인스턴스에서 합니다.

    ```bash
    mkdir -p /home/tomcat
    cp -a /usr/local/tomcat/conf /home/tomcat/conf
    ```

2. 상황에 맞는 요소를 추가 하 *server.xml* 내는 `<Server>` 요소입니다.

    ```xml
    <Server>
    ...
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ...
    </Server>
    ```

3. 응용 프로그램을 업데이트 *web.xml* 응용 프로그램에서 데이터 소스를 사용할 수 있습니다.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="finalize-configuration"></a>구성 완료

마지막으로 드라이버 Jar Tomcat classpath에 배치 하 고 App Service를 다시 시작 하십시오.

1. JDBC 드라이버 파일에 배치 하 여 Tomcat classloader를 사용할 수 있는지 확인 합니다 */home/tomcat/lib* 디렉터리입니다. (아직 존재하지 않는 경우 이 디렉터리를 만듭니다.) 이러한 파일을 App Service 인스턴스에 업로드하려면 다음 단계를 수행합니다.

    1. 에 [Cloud Shell](https://shell.azure.com), 웹 앱 확장을 설치 합니다.

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. App Service에 로컬 시스템에서 SSH 터널을 만들려면 다음 CLI 명령을 실행 합니다.

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. SFTP 클라이언트를 사용 하 여 로컬 터널링 포트에 연결 하 고 파일을 업로드 합니다 */home/tomcat/lib* 폴더입니다.

    또는 FTP 클라이언트를 사용하여 JDBC 드라이버를 업로드할 수 있습니다. [FTP 자격 증명을 가져오기 위한 이러한 지침](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)을 따릅니다.

2. 서버 수준 데이터 원본을 만든 경우 App Service Linux 애플리케이션을 다시 시작합니다. Tomcat이 `CATALINA_HOME`을 `/home/tomcat/conf`로 다시 설정하고 업데이트된 구성을 사용합니다.

### <a name="spring-boot"></a>Spring Boot

Spring Boot 응용 프로그램에서 데이터 원본에 연결할 것이 좋습니다 연결 문자열을 만들고에 주입 하 *application.properties* 파일입니다.

1. App Service 페이지의 "구성" 섹션에서는 문자열의 이름을 설정할 JDBC 연결 문자열 값 필드에 붙여 넣고 유형을 "Custom"으로 설정 합니다. 슬롯 설정으로 필요에 따라이 연결 문자열을 설정할 수 있습니다.

    이 연결 문자열은 라는 환경 변수로 응용 프로그램에 액세스할 수 있는 `CUSTOMCONNSTR_<your-string-name>`합니다. 위에서 만든 연결 문자열 이름은 예를 들어 `CUSTOMCONNSTR_exampledb`합니다.

2. 사용자 *application.properties* 파일에서이 연결 문자열 환경 변수 이름으로 참조 합니다. 예를 들어 다음 사용 됩니다.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

참조 하세요 합니다 [데이터 액세스에 대 한 Spring Boot 설명서](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) 및 [구성을 표면화](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) 이 항목에 대 한 자세한 내용은 합니다.

## <a name="configure-java-ee-wildfly"></a>구성 Java EE (WildFly)

> [!NOTE]
> App Service Linux에서 Java Enterprise Edition은 현재 미리 보기 중입니다. 이 스택이 **되지** 프로덕션 지향 작업에 대 한 것이 좋습니다. 이 Java SE 및 Tomcat 스택에서 정보입니다.

Linux의 azure App Service에 Java 개발자가 빌드 및 배포를 완전히 관리 되는 Linux 기반 서비스에 Java Enterprise (Java EE) 응용 프로그램을 확장할 수 있습니다.  기본 Java Enterprise 런타임 환경은 오픈 소스 [WildFly](https://wildfly.org/) 응용 프로그램 서버.

이 섹션은 다음 하위 섹션을 포함합니다.

- [App Service를 사용 하 여 크기 조정](#scale-with-app-service)
- [응용 프로그램 서버 구성을 사용자 지정](#customize-application-server-configuration)
- [모듈 및 종속성 설치](#install-modules-and-dependencies)
- [데이터 원본 구성](#configure-data-sources)
- [메시징 공급자를 사용 하도록 설정](#enable-messaging-providers)
- [세션 관리 캐싱을 구성 합니다.](#configure-session-management-caching)

### <a name="scale-with-app-service"></a>App Service의 크기 조정

Linux 기반의 App Service에서 실행 중인 WildFly 애플리케이션 서버는 도메인 구성이 아닌 독립 실행형 모드에서 실행됩니다. App Service 계획을 확장할 때 각 WildFly 인스턴스는 독립 실행형 서버로 구성됩니다.

[크기 조정 규칙](../../monitoring-and-diagnostics/monitoring-autoscale-get-started.md)을 사용하고 [인스턴스 수를 늘려서](../web-sites-scale.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) 애플리케이션을 수직 또는 수평적으로 확장합니다.

### <a name="customize-application-server-configuration"></a>사용자 지정 애플리케이션 서버 구성

웹 앱 인스턴스를 상태 비저장 되므로 각 새 인스턴스 시작 시 응용 프로그램에 필요한 WildFly 구성을 지원 하도록 구성 해야 합니다.
다음을 수행하기 위해 WildFly CLI를 호출하도록 시작 Bash 스크립트를 작성할 수 있습니다.

- 데이터 원본 설정
- 메시징 공급자 구성
- WildFly 서버 구성에 다른 모듈 및 종속성을 추가 합니다.

스크립트는 WildFly 작동 되어 실행 되지만 응용 프로그램이 시작 되기 전에 실행 됩니다. 스크립트를 사용 해야 합니다 [JBOSS CLI](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) 에서 호출 */opt/jboss/wildfly/bin/jboss-cli.sh* 구성 또는 서버에서 시작 된 후 필요한 변경 내용을 사용 하 여 응용 프로그램 서버를 구성 하려면.

WildFly를 구성 하는 대화형 모드의 CLI 사용 하지 마십시오. 대신 다음과 같은 `--file` 명령을 사용하여 JBoss CLI에 명령 스크립트를 제공할 수 있습니다.

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

FTP를 사용 하 여 시작 스크립트에서 App Service 인스턴스의 위치에 업로드 하 *home/* 와 같은 디렉터리 */home/site/deployments/tools*합니다. 자세한 내용은 참조 하세요. [FTP/S를 사용 하 여 Azure App Service에 앱 배포](https://docs.microsoft.com/azure/app-service/deploy-ftp)합니다.

설정 된 **시작 스크립트** 시작 셸 스크립트의 위치에 Azure portal에서 예를 들어 필드 */home/site/deployments/tools/your-startup-script.sh*합니다.

제공할 [앱 설정](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) 스크립트에서 사용 하기 위해 환경 변수를 전달 하 고 응용 프로그램 구성에서 합니다. 애플리케이션 설정은 연결 문자열 및 버전 제어에서 벗어나도록 애플리케이션을 구성하는 데 필요한 기타 비밀을 유지합니다.

### <a name="install-modules-and-dependencies"></a>모듈 및 종속성 설치

모듈 및 해당 종속성 JBoss CLI를 통해 WildFly classpath에를 설치 하려면 해당 디렉터리에서 다음 파일을 작성 해야 합니다. 일부 모듈 및 종속성에는 JNDI 명명 또는 기타 API 관련 구성과 같은 추가 구성이 필요할 수 있습니다. 따라서 이 목록은 대부분의 경우에서 종속성을 구성해야 하는 최소 세트입니다.

- [XML 모듈 설명자](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). 이 XML 파일은 모듈의 이름, 특성 및 종속성을 정의합니다. 이 [예제 module.xml 파일](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource)은 Postgres 모듈, 해당 JAR 파일 JDBC 종속성 및 필요한 기타 모듈 종속성을 정의합니다.
- 사용자 모듈에 대한 필수 JAR 파일 종속성.
- 새 모듈을 구성하기 위한 JBoss CLI 명령을 사용하는 스크립트. 이 파일은 종속성을 사용하도록 서버를 구성하기 위해 JBoss CLI에서 실행할 명령을 포함합니다. 모듈, 데이터 원본 및 메시징 공급자를 추가하는 명령에 대한 문서는 [이 문서](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli)를 참조하세요.
- JBoss CLI를 호출하고 이전 단계에서 스크립트를 실행하는 Bash 시작 스크립트. 이 파일은 스케일 아웃을 수행하는 동안 App Service 인스턴스를 다시 시작하거나 새 인스턴스를 프로비전할 때 실행됩니다. 이 시작 스크립트는 JBoss 명령이 JBoss CLI로 전달되므로 애플리케이션에 대해 다른 구성을 수행할 수 있습니다. 최소한 이 파일은 JBoss CLI 명령 스크립트를 JBoss CLI에 전달하는 단일 명령일 수 있습니다.

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

모듈에 대 한 파일 및 콘텐츠를 만든 후 WildFly 응용 프로그램 서버에 모듈을 추가 하려면 다음 단계를 수행 합니다.

1. FTP를 사용 하 여 파일에서 App Service 인스턴스의 위치에 업로드 하 *홈/* 와 같은 디렉터리 */home/site/deployments/tools*합니다. 자세한 내용은 참조 하세요. [FTP/S를 사용 하 여 Azure App Service에 앱 배포](../deploy-ftp.md)합니다.
2. 에 **구성** > **일반 설정** Azure 포털의 페이지 설정 된 **시작 스크립트** 에 대 한 필드 시작 셸 스크립트의 위치를 예제 */home/site/deployments/tools/startup.sh*합니다.
3. 키를 눌러 App Service 인스턴스를 다시 시작 합니다 **다시 시작** 단추를 **개요** 포털 또는 Azure CLI를 사용 하는 섹션입니다.

### <a name="configure-data-sources"></a>데이터 원본 구성

데이터 원본에 액세스 하려면 WildFly/JBoss를 구성 하려면 "설치 모듈 및 종속성" 섹션에서 위에 설명 된 일반 프로세스를 사용할 수 있습니다. 다음 섹션에서는 PostgreSQL, MySQL 및 SQL Server 데이터 원본에 대해이 프로세스에 특정 세부 정보를 제공합니다.

이 섹션에서는 앱, App Service 인스턴스 및 Azure Database 서비스 인스턴스에 이미 있는 것을 가정 합니다. 아래 지침에 따라 App Service 이름, 해당 리소스 그룹 및 데이터베이스 연결 정보를 가리킵니다. Azure portal에서이 정보를 찾을 수 있습니다.

내용은 샘플 앱을 사용 하 여 처음부터 전체 프로세스를 진행 하려는 경우 [자습서: Azure에서 Java EE 및 Postgres 웹 앱 빌드](tutorial-java-enterprise-postgresql-app.md)합니다.

다음 단계는 기존 App Service 및 데이터베이스에 연결 하기 위한 요구 사항을 설명 합니다.

1. JDBC 드라이버를 다운로드 [PostgreSQL](https://jdbc.postgresql.org/download.html)를 [MySQL](https://dev.mysql.com/downloads/connector/j/), 또는 [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)합니다. 가져올 드라이버.jar 파일을 다운로드 한 보관 압축을 풉니다.

2. 같은 이름의 파일을 만듭니다 *module.xml* 다음 태그를 추가 합니다. 대체는 `<module name>` 자리 표시자 (꺾쇠 괄호를 포함)를 `org.postgres` PostgreSQL에 대 한 `com.mysql` MySQL에 대 한 또는 `com.microsoft` SQL Server에 대 한 합니다. 대체 `<JDBC .jar file path>` 이전 단계에서.jar 파일 이름의 전체 경로 위치를 포함 하 여에 배치 하는 파일을 App Service 인스턴스. 아래에 있는 모든 위치가 될 수 있으며이 *home/* 디렉터리입니다.

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="<module name>">
        <resources>
           <resource-root path="<JDBC .jar file path>" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

3. 같은 이름의 파일을 만듭니다 *datasource commands.cli* 다음 코드를 추가 합니다. 대체 `<JDBC .jar file path>` 이전 단계에서 사용 된 값입니다. 바꿉니다 `<module file path>` 예를 들어 이전 단계에서 App Service 경로 파일 이름을 사용 하 여 */home/module.xml*합니다.

    **PostgreSQL**

    ```console
    module add --name=org.postgres --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=postgres:add(driver-name=postgres,driver-module-name=org.postgres,driver-class-name=org.postgresql.Driver,driver-xa-datasource-class-name=org.postgresql.xa.PGXADataSource)

    data-source add --name=postgresDS --driver-name=postgres --jndi-name=java:jboss/datasources/postgresDS --connection-url=$DATABASE_CONNECTION_URL --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=org.postgresql.Driver --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter --jta=true --use-java-context=true --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker

    reload --use-current-server-config=true
    ```

    **MySQL**

    ```console
    module add --name=com.mysql --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql,driver-class-name=com.mysql.cj.jdbc.Driver)

    data-source add --name=mysqlDS --jndi-name=java:jboss/datasources/mysqlDS --connection-url=$DATABASE_CONNECTION_URL --driver-name=mysql --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=com.mysql.cj.jdbc.Driver --jta=true --use-java-context=true --exception-sorter-class-name=com.mysql.cj.jdbc.integration.jboss.ExtendedMysqlExceptionSorter

    reload --use-current-server-config=true
    ```

    **SQL Server**

    ```console
    module add --name=com.microsoft --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=sqlserver:add(driver-name=sqlserver,driver-module-name=com.microsoft,driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver,driver-datasource-class-name=com.microsoft.sqlserver.jdbc.SQLServerDataSource)

    data-source add --name=sqlDS --jndi-name=java:jboss/datasources/sqlDS --driver-name=sqlserver --connection-url=$DATABASE_CONNECTION_URL --validate-on-match=true --background-validation=false --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLValidConnectionChecker --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLExceptionSorter

    reload --use-current-server-config=true
    ```

    이 파일은 다음 단계에 설명 된 시작 스크립트가 실행 됩니다. WildFly 모듈로 JDBC 드라이버를 설치, 해당 WildFly 데이터 원본을 만들고 변경 내용을 적용할 수 있도록 서버를 다시 로드 합니다.

4. 같은 이름의 파일을 만듭니다 *startup.sh* 다음 코드를 추가 합니다. 대체 `<JBoss CLI script>` 이전 단계에서 만든 파일의 이름입니다. 전체 경로 포함 해야 합니다. 위치에 배치 하는 파일을 App Service 인스턴스, 예를 들어 */home/datasource-commands.cli*합니다.

    ```bash
    #!/usr/bin/env bash
    /opt/jboss/wildfly/bin/jboss-cli.sh -c --file=<JBoss CLI script>
    ```

5. FTP를 사용 하 여 App Service에 JDBC.jar 파일, 모듈 XML 파일, JBoss CLI 스크립트 및 시작 스크립트를 업로드 합니다. 와 같은 이전 단계에서 지정한 위치에 이러한 파일 보관 *home/* 합니다. FTP에 대 한 자세한 내용은 참조 하세요. [FTP/S를 사용 하 여 Azure App Service에 앱 배포](https://docs.microsoft.com/azure/app-service/deploy-ftp)합니다.

6. Azure CLI를 사용 하 여 데이터베이스 연결 정보를 포함 하는 App Service에 설정을 추가 합니다. 대체 `<resource group>` 고 `<webapp name>` 값을 사용 하 여 App Service를 사용 합니다. 바꿉니다 `<database server name>`, `<database name>`를 `<admin name>`, 및 `<admin password>` 데이터베이스 연결 정보를 사용 하 여 합니다. Azure portal에서 App Service 및 데이터베이스 정보를 가져올 수 있습니다.

    **PostgreSQL:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:postgresql://<database server name>:5432/<database name>?ssl=true \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **MySQL:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **SQL Server:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
    ```

    DATABASE_CONNECTION_URL 값은 각 데이터베이스 서버에 대해 다른 및 Azure portal에 있는 값과 다릅니다. 여기와 위 코드 조각에 표시 된 URL 형식이 WildFly에서 사용 하기 위해 필요 합니다.

    * **PostgreSQL:** `jdbc:postgresql://<database server name>:5432/<database name>?ssl=true`
    * **MySQL:** `jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT`
    * **SQL Server:** `jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;`

7. 찾아 Azure portal에서 App Service로 이동 합니다 **Configuration** > **일반 설정** 페이지입니다. 설정 된 **시작 스크립트** 예를 들어 필드 이름 및 시작 스크립트의 위치를 */home/startup.sh*합니다.

App Service는 다음 작업을 다시 시작 되 면 다음에 시작 스크립트를 실행 하 고 필요한 구성 단계를 수행 합니다. 이 구성을 올바르게 수행을 테스트 하려면 SSH를 사용 하 여 App Service에 액세스할 수 있으며 다음 시작 스크립트를 직접 실행할 Bash 프롬프트에서 키를 누릅니다. 또한 App Service 로그를 검사할 수 있습니다. 이러한 옵션에 대 한 자세한 내용은 참조 하세요. [로깅 및 앱 디버깅](#logging-and-debugging-apps)합니다.

다음으로 앱에 대 한 WildFly 구성을 업데이트 및 다시 배포 해야 합니다. 다음 단계를 사용합니다.

1. 엽니다는 *src/main/resources/META-INF/persistence.xml* 앱과 찾기에 대 한 파일을 `<jta-data-source>` 요소. 다음과 같이 해당 내용을 바꿉니다.

    **PostgreSQL**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

    **MySQL**

    ```xml
    <jta-data-source>java:jboss/datasources/mysqlDS</jta-data-source>
    ```

    **SQL Server**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

2. 다시 빌드하고 다시 Bash 프롬프트에서 다음 명령을 사용 하 여 앱을 배포 합니다.

    ```bash
    mvn package -DskipTests azure-webapp:deploy
    ```

3. 키를 눌러 App Service 인스턴스를 다시 시작 합니다 **다시 시작** 단추를 **개요** 섹션에서는 Azure portal 또는 Azure CLI를 사용 하 여 합니다.

App Service 인스턴스는 이제 데이터베이스에 액세스 하도록 구성 됩니다.

WildFly를 사용 하 여 데이터베이스 연결 구성에 대 한 자세한 내용은 참조 하세요. [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7)하십시오 [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource), 또는 [SQL Server](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898)합니다.

### <a name="enable-messaging-providers"></a>메시징 공급자를 사용 하도록 설정

Service Bus를 메시징 메커니즘으로 사용하여 메시지 기반 Bean을 사용하도록 설정하려면

1. [Apache QPId JMS 메시징 라이브러리](https://qpid.apache.org/proton/index.html)를 사용합니다. 애플리케이션에 대한 pom.xml(또는 다른 빌드 파일)에 이 종속성을 포함시킵니다.

2. [Service Bus 리소스](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)를 만듭니다. 보내기 및 받기 기능을 사용하여 해당 네임스페이스 및 공유 액세스 정책 내에 Azure Service Bus 네임스페이스 및 큐를 만듭니다.

3. 정책의 기본 키를 URL로 인코딩하거나 [Service Bus SDK](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory)를 사용하여 공유 액세스 정책 키를 코드에 전달합니다.

4. JMS 공급자에 대한 모듈 XML 설명자, .jar 종속성, JBoss CLI 명령 및 시작 스크립트를 사용하여 모듈 설치 및 종속성 섹션에 설명된 단계를 따릅니다. 4개의 파일 외에도 JMS 큐 및 토픽에 대한 JNDI 이름을 정의하는 XML 파일을 만들어야 합니다. 참조 구성 파일은 [이 리포지토리](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig)를 참조하세요.

### <a name="configure-session-management-caching"></a>세션 관리 캐싱 구성

기본적으로 Linux의 App Service는 기존 세션을 사용하는 클라이언트 요청이 동일한 애플리케이션 인스턴스로 라우팅되도록 세션 선호도 쿠키를 사용합니다. 이 기본 동작에는 구성이 필요하지 않지만, 다음과 같은 몇 가지 제한 사항이 있습니다.

- 애플리케이션 인스턴스가 다시 시작하거나 규모가 축소되면 애플리케이션 서버의 사용자 세션 상태가 손실됩니다.
- 애플리케이션의 세션 시간 초과 설정이 길거나 사용자 수가 고정된 경우 새 세션만 새로 시작된 인스턴스로 라우팅되므로 자동으로 크기가 조정된 새 인스턴스가 로드를 받는 데 시간이 다소 걸릴 수 있습니다.

와 같은 외부 세션 저장소를 사용 하는 WildFly를 구성할 수 있습니다 [redis Azure Cache](/azure/azure-cache-for-redis/)합니다. 해야 [기존 ARR 인스턴스 선호도 사용 하지 않도록 설정](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) 세션 쿠키 기반 라우팅을 해제 문제 없이 작동 하도록 구성 된 WildFly 세션 저장소를 허용 하는 구성 합니다.

## <a name="docker-containers"></a>Docker 컨테이너

컨테이너에 Azure 지원 Zulu JDK를 사용하려면 [Azure에 지원되는 Azul Zulu Enterprise 다운로드 페이지](https://www.azul.com/downloads/azure-only/zulu/)에 나와 있는 미리 작성된 이미지를 끌어와서 사용하거나 [Microsoft Java GitHub 리포지토리](https://github.com/Microsoft/java/tree/master/docker)의 `Dockerfile` 예제를 사용하세요.

## <a name="statement-of-support"></a>지원 정보

### <a name="runtime-availability"></a>런타임 가용성

Linux용 App Service는 Java 웹 애플리케이션의 관리되는 호스팅에 다음과 같은 두 가지 런타임을 지원합니다.

- 웹 보관(WAR) 파일로 패키징된 애플리케이션을 실행하기 위한 [Tomcat 서블릿 컨테이너](https://tomcat.apache.org/). 지원되는 버전은 8.5 및 9.0입니다.
- Java 보관(JAR) 파일로 패키징된 애플리케이션을 실행하기 위한 Java SE 런타임 환경. 지원 되는 버전은 Java 8 및 11입니다.

### <a name="jdk-versions-and-maintenance"></a>JDK 버전 및 유지 관리

OpenJDK의 Azul Zulu Enterprise 빌드는 Microsoft와 Azul Systems가 후원하는 Azure 및 Azure Stack에 대한 OpenJDK의 무료 다중 플랫폼 프로덕션 준비 배포입니다. 여기에는 Java SE 애플리케이션을 빌드하고 실행하기 위한 모든 구성 요소가 포함됩니다. JDK를 설치할 수 있습니다 [Java JDK 설치](https://aka.ms/azure-jdks)합니다.

지원되는 JDK는 매년 분기마다 1월, 4월, 7월, 10월에 자동으로 패치됩니다.

### <a name="security-updates"></a>보안 업데이트

Azul Systems에서 주요 보안 취약점에 대한 패치 및 수정 사항을 출시하는 즉시 고객에게 제공됩니다. [NIST Common Vulnerability Scoring System 버전 2](https://nvd.nist.gov/cvss.cfm)에서 기본 점수 9.0 이상을 받으면 "주요" 취약점으로 정의됩니다.

### <a name="deprecation-and-retirement"></a>사용 중단 및 사용 중지

지원되는 Java 런타임이 폐기되는 경우 폐기 예정인 런타임을 사용하는 Azure 개발자에게는 적어도 런타임 폐기 6개월 전에 사용 중단 알림이 제공됩니다.

## <a name="next-steps"></a>다음 단계

[Java 개발자용 Azure](/java/azure/) 센터를 방문하여 Azure 빠른 시작, 자습서 및 Java 참조 설명서를 찾아보세요.

Java 개발에 국한되지 않는 Linux용 App Service 사용에 대한 일반적인 질문의 답은 [App Service Linux FAQ](app-service-linux-faq.md)에서 찾을 수 있습니다.

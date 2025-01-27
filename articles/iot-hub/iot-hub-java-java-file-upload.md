---
title: Java를 사용하여 디바이스에서 Azure IoT Hub로 파일 업로드 | Microsoft Docs
description: Java용 Azure IoT 디바이스 SDK를 사용하여 디바이스에서 클라우드로 파일을 업로드하는 방법입니다. 업로드된 파일은 Azure Storage blob 컨테이너에 저장됩니다.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 06/28/2017
ms.openlocfilehash: 27cdada0bfbb4236e16d17c263aaba0f4f5c511f
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620125"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>IoT Hub를 사용하여 디바이스에서 클라우드로 파일 업로드

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

코드를 기반으로 한이 자습서는 [IoT Hub를 사용 하 여 클라우드-장치 메시지 보내기](iot-hub-java-java-c2d.md) 자습서를 사용 하는 방법을 보여는 [IoT Hub의 업로드 기능 파일](iot-hub-devguide-file-upload.md) 파일을 업로드할 [Azure blob 저장소](../storage/index.yml)합니다. 이 자습서에서는 다음을 수행하는 방법에 대해 설명합니다.

* 파일을 업로드하기 위한 Azure blob URI를 디바이스에 안전하게 제공합니다.

* IoT Hub 파일 업로드 알림을 사용하여 앱 백 엔드에서 파일 처리를 트리거합니다.

합니다 [IoT hub에 장치에서 원격 분석을 보내고](quickstart-send-telemetry-java.md) 빠른 시작 및 [IoT Hub를 사용 하 여 클라우드-장치 메시지 보내기](iot-hub-java-java-c2d.md) 자습서 IoT의 기본적인 장치-클라우드 및 클라우드-장치 메시징 기능을 보여 줍니다. 허브입니다. [IoT Hub로 메시지 라우팅 구성](tutorial-routing.md) 자습서에서는 장치-클라우드 메시지를 Azure Blob Storage에 안정적으로 저장하는 방법에 대해 설명합니다. 그러나 일부 시나리오에서는 디바이스에서 전송하는 데이터를 IoT Hub에서 허용하는 비교적 작은 디바이스-클라우드 메시지에 쉽게 매핑할 수 없습니다. 예:

* 이미지가 포함된 대형 파일
* 비디오
* 자주 샘플링되는 진동 데이터
* 특정 형태의 전처리된 데이터

이러한 파일은 일반적으로 [Azure Data Factory](../data-factory/introduction.md) 또는 [Hadoop](../hdinsight/index.yml) 스택과 같은 도구를 사용하여 클라우드에서 배치 방식으로 처리됩니다. 디바이스에서 파일을 업로드해야 할 때 IoT Hub의 보안 및 안정성을 여전히 사용할 수 있습니다.

이 자습서의 끝 부분에서는 다음 두 개의 Java 콘솔 앱을 실행합니다.

* **시뮬레이션 된 장치**을 [IoT Hub를 사용 하 여 송신 클라우드-장치 메시지] 자습서에서 만든 앱의 수정된 된 버전입니다. 이 앱은 IoT Hub에서 제공하는 SAS URI를 사용하여 저장소에 파일을 업로드합니다.

* **read-file-upload-notification** - IoT Hub에서 파일 업로드 알림을 받습니다.

> [!NOTE]
> IoT Hub는 Azure IoT 디바이스 SDK를 통해 많은 디바이스 플랫폼 및 언어(C, .NET 및 Javascript 포함)를 지원합니다. Azure IoT Hub에 디바이스를 연결하는 방법에 대한 단계별 지침은 [Azure IoT 개발자 센터](https://azure.microsoft.com/develop/iot)를 참조하세요.

이 자습서를 완료하려면 다음이 필요합니다.

* 최신 [Java SE Development Kit 8](https://aka.ms/azure-jdks)

* [Maven 3](https://maven.apache.org/install.html)

* 활성 Azure 계정. 계정이 없는 경우 몇 분 만에 [무료 계정](https://azure.microsoft.com/pricing/free-trial/)을 만들 수 있습니다.

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>디바이스 앱에서 파일 업로드

이 섹션에서는에서 만든 장치 앱을 수정 하 [IoT Hub를 사용 하 여 클라우드-장치 메시지 보내기](iot-hub-java-java-c2d.md) IoT hub에 파일을 업로드 합니다.

1. 이미지 파일을 `simulated-device` 폴더에 복사하고 파일 이름을 `myimage.png`로 바꿉니다.

2. 텍스트 편집기를 사용하여 `simulated-device\src\main\java\com\mycompany\app\App.java` 파일을 엽니다.

3. **App** 클래스에 변수 선언을 추가합니다.

    ```java
    private static String fileName = "myimage.png";
    ```

4. 파일 업로드 상태 콜백 메시지를 처리하려면 다음 중첩 클래스를 **App** 클래스에 추가합니다.

    ```java
    // Define a callback method to print status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to file upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

5. 이미지를 IoT Hub에 업로드하려면 IoT Hub에 이미지를 업로드하도록 다음 메서드를 **App** 클래스에 추가합니다.

    ```java
    // Use IoT Hub to upload a file asynchronously to Azure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

6. 다음 코드 조각에 나와 있는 것처럼, **uploadFile** 메서드를 호출하도록 **main** 메서드를 수정합니다.

    ```java
    client.open();

    try
    {
      // Get the filename and start the upload.
      String fullFileName = System.getProperty("user.dir") + File.separator + fileName;
      uploadFile(fullFileName);
      System.out.println("File upload started with success");
    }
    catch (Exception e)
    {
      System.out.println("Exception uploading file: " + e.getCause() + " \nERROR: " + e.getMessage());
    }

    MessageSender sender = new MessageSender();
    ```

7. 다음 명령을 사용하여 **simulated-device** 앱을 작성하고 오류가 있는지 확인합니다.

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a>파일 업로드 알림 수신

이 섹션에서는 IoT Hub에서 파일 업로드 알림 메시지를 수신하는 Java 콘솔 앱을 만듭니다.

IoT Hub가 이 섹션을 완료하려면 **iothubowner** 연결 문자열이 필요합니다. 이 연결 문자열은 [Azure Portal](https://portal.azure.com/)에서 찾거나 **공유 액세스 정책** 블레이드에서 찾을 수 있습니다.

1. 명령 프롬프트에서 다음 명령을 사용하여 **read-file-upload-notification**이라는 Maven 프로젝트를 만듭니다. 이 명령은 긴 단일 명령입니다.

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. 명령 프롬프트에서 새 `read-file-upload-notification` 폴더로 이동합니다.

3. 텍스트 편집기를 사용하여 `read-file-upload-notification` 폴더에서 `pom.xml` 파일을 열고 **종속성** 노드에 다음 종속성을 추가합니다. 의존성을 추가하면 IoT Hub 서비스와 통신하기 위해 애플리케이션에서 **iothub-java-service-client** 패키지를 사용할 수 있습니다.

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > [Maven 검색](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22)을 사용하여 **iot-service-client**의 최신 버전을 확인할 수 있습니다.

4. `pom.xml` 파일을 저장하고 닫습니다.

5. 텍스트 편집기를 사용하여 `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` 파일을 엽니다.

6. 파일에 다음 **import** 문을 추가합니다.

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

7. 다음 클래스 수준 변수를 **앱** 클래스에 추가합니다.

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

8. 콘솔에 파일을 업로드하는 정보를 인쇄하려면 다음 중첩 클래스를 **App** 클래스에 추가합니다.

    ```java
    // Create a thread to receive file upload notifications.
    private static class ShowFileUploadNotifications implements Runnable {
      public void run() {
        try {
          while (true) {
            System.out.println("Receive file upload notifications...");
            FileUploadNotification fileUploadNotification = fileUploadNotificationReceiver.receive();
            if (fileUploadNotification != null) {
              System.out.println("File Upload notification received");
              System.out.println("Device Id : " + fileUploadNotification.getDeviceId());
              System.out.println("Blob Uri: " + fileUploadNotification.getBlobUri());
              System.out.println("Blob Name: " + fileUploadNotification.getBlobName());
              System.out.println("Last Updated : " + fileUploadNotification.getLastUpdatedTimeDate());
              System.out.println("Blob Size (Bytes): " + fileUploadNotification.getBlobSizeInBytes());
              System.out.println("Enqueued Time: " + fileUploadNotification.getEnqueuedTimeUtcDate());
            }
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

9. 파일 업로드 알림을 수신하는 스레드를 시작하려면 다음 코드를 **main** 메서드에 추가합니다.

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from the ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start the thread to receive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER to exit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

10. `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` 파일을 저장하고 닫습니다.

11. 다음 명령을 사용하여 **read-file-upload-notification** 앱을 작성하고 오류가 있는지 확인합니다.

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>애플리케이션 실행

이제 애플리케이션을 실행할 준비가 되었습니다.

`read-file-upload-notification` 폴더의 명령 프롬프트에서 다음 명령을 실행합니다.

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

`simulated-device` 폴더의 명령 프롬프트에서 다음 명령을 실행합니다.

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

다음 스크린샷은 **simulated-device** 앱의 출력을 보여 줍니다.

![simulated-device 앱의 출력](media/iot-hub-java-java-upload/simulated-device.png)

다음 스크린샷은 **read-file-upload-notification** 앱의 출력을 보여 줍니다.

![read-file-upload-notification 앱의 출력](media/iot-hub-java-java-upload/read-file-upload-notification.png)

포털을 사용하면 구성한 저장소 컨테이너에 업로드된 파일을 볼 수 있습니다.

![업로드된 파일](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a>다음 단계

이 자습서에서는 디바이스에서 파일 업로드를 단순화하기 위해 IoT Hub의 파일 업로드 기능을 사용하는 방법을 알아보았습니다. 다음 문서를 사용하여 IoT Hub 기능 및 시나리오를 계속 탐색할 수 있습니다.

* [프로그래밍 방식으로 IoT Hub 만들기](iot-hub-rm-template-powershell.md)

* [C SDK 소개](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK](iot-hub-devguide-sdks.md)

IoT Hub의 기능을 추가로 탐색하려면 다음을 참조하세요.

* [IoT Edge에서 디바이스 시뮬레이션](../iot-edge/tutorial-simulate-device-linux.md)

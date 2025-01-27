---
title: '빠른 시작: Anomaly Detector REST API와 Java를 사용하여 시계열 데이터에서 변칙 검색'
titleSuffix: Azure Cognitive Services
description: Anomaly Detector API를 사용하여 일괄 처리 또는 스트리밍 데이터로 데이터 계열에서 변칙을 검색합니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 2a219dfac597208a2c409f76c035a1b913864245
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721512"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-java"></a>빠른 시작: Anomaly Detector REST API와 Java를 사용하여 시계열 데이터에서 변칙 검색

이 빠른 시작을 통해 Anomaly Detector API의 두 가지 검색 모드를 사용하여 시계열 데이터에서 변칙을 검색합니다. 이 Java 애플리케이션은 JSON 형식의 시계열 데이터가 포함된 2개의 API 요청을 보내고 응답을 받습니다.

| API 요청                                        | 애플리케이션 출력                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| 일괄 처리로 변칙 검색                        | 시계열 데이터의 각 데이터 요소에 대한 변칙 상태(및 기타 데이터)와 검색된 변칙의 위치가 포함된 JSON 응답입니다. |
| 최신 데이터 요소의 변칙 상태 검색 | 시계열 데이터의 최신 데이터 요소에 대한 변칙 상태(및 기타 데이터)가 포함된 JSON 응답입니다.                                                                                                                                         |

 이 애플리케이션은 Java로 작성되었지만, API는 대부분의 프로그래밍 언어와 호환되는 RESTful 웹 서비스입니다.

## <a name="prerequisites"></a>필수 조건

- [Java&trade; Development Kit(JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 이상.

- Maven 리포지토리에서 다음과 같은 라이브러리 가져오기
    - [JSON(Java](https://mvnrepository.com/artifact/org.json/json) 패키지)
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) 패키지

- 시계열 데이터 요소가 포함된 JSON 파일 이 빠른 시작의 예제 데이터는 [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json)에서 확인할 수 있습니다.

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

## <a name="create-a-new-application"></a>새 애플리케이션 만들기

1. 즐겨 찾는 IDE 또는 편집기에서 새 Java 프로젝트를 만들고 다음 라이브러리를 가져옵니다.

    ```java
    import org.apache.http.HttpEntity;
    import org.apache.http.client.methods.CloseableHttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClients;
    import org.apache.http.util.EntityUtils;
    import org.json.JSONArray;
    import org.json.JSONObject;
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Paths;
    ```

2. 구독 키 및 엔드포인트에 대한 변수를 만듭니다. 아래는 변칙 검색에 사용할 수 있는 URI입니다. 나중에 API 요청 URL을 만드는 서비스 엔드포인트에 추가됩니다.

    |검색 방법  |URI  |
    |---------|---------|
    |일괄 검색    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |최신 데이터 요소에서 검색     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    ```java
    // Replace the subscriptionKey string value with your valid subscription key.
    static final String subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";
    //replace the endpoint URL with the correct one for your subscription. Your endpoint can be found in the Azure portal. 
    //For example: https://westus2.api.cognitive.microsoft.com
    static final String endpoint = "[YOUR_ENDPOINT_URL]";
    // Replace the dataPath string with a path to the JSON formatted time series data.
    static final String dataPath = "[PATH_TO_TIME_SERIES_DATA]";
    static final String latestPointDetectionUrl = "/anomalydetector/v1.0/timeseries/last/detect";
    static final String batchDetectionUrl = "/anomalydetector/v1.0/timeseries/entire/detect";
    ```

3. JSON 데이터 파일에서 읽기

    ```java
    String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "utf-8");
    ```

## <a name="create-a-function-to-send-requests"></a>요청을 보내는 함수 만들기

1. 위에서 만든 변수를 사용하는 `sendRequest()`라는 새 함수를 만듭니다. 그런 후에 다음 단계를 수행합니다.

2. API에 요청을 보낼 수 있는 `CloseableHttpClient` 개체를 만듭니다. 엔드포인트와 Anomaly Detector URL을 결합하여 `HttpPost` 요청 개체에 요청을 보냅니다.

3. 요청의 `setHeader()` 함수를 사용하여 `Content-Type` 헤더를 `application/json`으로 설정하고 구독 키를 `Ocp-Apim-Subscription-Key` 헤더에 추가합니다.

4. 요청의 `setEntity()` 함수를 전송할 데이터에 사용합니다.

5. 클라이언트의 `execute()` 함수를 사용하여 요청을 보내고 `CloseableHttpResponse` 개체에 저장합니다.

6. 응답 콘텐츠를 저장할 `HttpEntity` 개체를 만듭니다. `getEntity()`를 사용하여 콘텐츠를 받습니다. 응답이 비어 있지 않으면 반환합니다.

```java
static String sendRequest(String apiAddress, String endpoint, String subscriptionKey, String requestData) {
    try (CloseableHttpClient client = HttpClients.createDefault()) {
        HttpPost request = new HttpPost(endpoint + apiAddress);
        // Request headers.
        request.setHeader("Content-Type", "application/json");
        request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
        request.setEntity(new StringEntity(requestData));
        try (CloseableHttpResponse response = client.execute(request)) {
            HttpEntity respEntity = response.getEntity();
            if (respEntity != null) {
                return EntityUtils.toString(respEntity, "utf-8");
            }
        } catch (Exception respEx) {
            respEx.printStackTrace();
        }
    } catch (IOException ex) {
        System.err.println("Exception on Anomaly Detector: " + ex.getMessage());
        ex.printStackTrace();
    }
    return null;
}
```

## <a name="detect-anomalies-as-a-batch"></a>일괄 처리로 변칙 검색

1. `detectAnomaliesBatch()`라는 메서드를 만들어서 데이터 전체의 변칙을 일괄 처리로 검색합니다. 위에서 만든 `sendRequest()` 메서드를 엔드포인트, URL, 구독 키 및 JSON 데이터로 호출합니다. 결과를 받아서 콘솔에 출력합니다.

2. 응답에 `code` 필드가 포함된 경우에는 오류 코드 및 오류 메시지를 출력합니다.

3. 그렇지 않으면 데이터 세트에서 변칙의 위치를 찾습니다. 응답의 `isAnomaly` 필드는 지정된 데이터 요소가 변칙인지 여부와 관련된 부울 값을 포함합니다. JSON 배열을 가져오고 반복하여 `true` 값의 인덱스를 출력합니다. 이러한 값은 변칙 데이터 요소의 인덱스와 일치합니다(발견된 경우).

```java
static void detectAnomaliesBatch(String requestData) {
    System.out.println("Detecting anomalies as a batch");
    String result = sendRequest(batchDetectionUrl, endpoint, subscriptionKey, requestData);
    if (result != null) {
        System.out.println(result);
        JSONObject jsonObj = new JSONObject(result);
        if (jsonObj.has("code")) {
            System.out.println(String.format("Detection failed. ErrorCode:%s, ErrorMessage:%s", jsonObj.getString("code"), jsonObj.getString("message")));
        } else {
            JSONArray jsonArray = jsonObj.getJSONArray("isAnomaly");
            System.out.println("Anomalies found in the following data positions:");
            for (int i = 0; i < jsonArray.length(); ++i) {
                if (jsonArray.getBoolean(i))
                    System.out.print(i + ", ");
            }
            System.out.println();
        }
    }
}
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>최신 데이터 요소의 변칙 상태 검색

* `detectAnomaliesLatest()`라는 메서드를 만들어서 데이터 세트에서 마지막 데이터 요소의 비정상 상태를 검색합니다. 위에서 만든 `sendRequest()` 메서드를 엔드포인트, URL, 구독 키 및 JSON 데이터로 호출합니다. 결과를 받아서 콘솔에 출력합니다.

```java
static void detectAnomaliesLatest(String requestData) {
    System.out.println("Determining if latest data point is an anomaly");
    String result = sendRequest(latestPointDetectionUrl, endpoint, subscriptionKey, requestData);
    System.out.println(result);
}
```

## <a name="load-your-time-series-data-and-send-the-request"></a>시계열 데이터 로드 및 요청 보내기

1. 애플리케이션의 main 메서드에서 요청에 추가할 데이터가 포함된 JSON 파일을 읽습니다.

2. 위에서 만든 변칙 검색 함수를 호출합니다.

```java
public static void main(String[] args) throws Exception {
    String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "utf-8");
    detectAnomaliesBatch(requestData);
    detectAnomaliesLatest(requestData);
}
```

### <a name="example-response"></a>예제 응답

성공 응답이 JSON 형식으로 반환됩니다. 아래 링크를 클릭하면 GitHub에서 JSON 응답을 볼 수 있습니다.
* [일괄 검색 응답 예제](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [최신 요소 검색 응답 예제](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [REST API 참조](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)

---
title: 웹 서비스 배포 사용 방법 - Azure Machine Learning
description: Azure Machine Learning 모델을 배포하여 만든 웹 서비스를 사용하는 방법을 알아봅니다. Azure Machine Learning 모델을 배포하면 REST API를 노출하는 웹 서비스가 생성됩니다. 선택한 프로그래밍 언어를 사용하여 이 API에 대한 클라이언트를 만들 수 있습니다. 이 문서에서는 Python 및 C#을 사용하여 API에 액세스하는 방법을 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
ms.reviewer: larryfr
ms.date: 10/30/2018
ms.openlocfilehash: 75faf344c64dc330a98b836a8852b42531645c49
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51685177"
---
# <a name="consume-an-azure-machine-learning-model-deployed-as-a-web-service"></a>웹 서비스로 배포된 Azure Machine Learning 모델 사용

Azure Machine Learning 모델을 웹 서비스로 배포하면 REST API가 생성됩니다. 이 API로 데이터를 보내고 모델에서 반환된 예측을 받을 수 있습니다. 이 문서에서는 C#, Go, Java 및 Python을 사용하여 웹 서비스용 클라이언트를 만드는 방법에 대해 알아봅니다.

Azure Container 인스턴스, Azure Kubernetes Service 또는 Project Brainwave(필드 프로그래밍 가능 게이트 배열)에 이미지를 배포할 때 웹 서비스가 생성됩니다. 이미지는 등록된 모델과 채점 파일에서 생성됩니다. 웹 서비스에 액세스하는 데 사용되는 URI는 [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)를 사용하여 검색할 수 있습니다. 인증이 사용 가능한 경우 SDK를 사용하여 인증 키를 가져올 수도 있습니다.

ML 웹 서비스를 사용하는 클라이언트를 만들 때 일반적인 워크플로는 다음과 같습니다.

1. SDK를 사용하여 연결 정보 가져오기
1. 모델에서 사용하는 요청 데이터 형식 결정
1. 웹 서비스를 호출하는 애플리케이션 만들기

## <a name="connection-information"></a>연결 정보

> [!NOTE]
> Azure Machine Learning SDK는 웹 서비스 정보를 가져오는 데 사용됩니다. 이는 Python SDK입니다. 웹 서비스에 대한 정보를 검색하는 데 사용되는 동안 모든 언어를 사용하여 서비스용 클라이언트를 만들 수 있습니다.

웹 서비스 연결 정보는 Azure Machine Learning SDK를 사용하여 검색할 수 있습니다. [azureml.core.Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) 클래스는 클라이언트를 만드는 데 필요한 정보를 제공합니다. 다음 `Webservice` 속성은 클라이언트 애플리케이션을 만들 때 유용합니다.

* `auth_enabled` - 인증을 사용하도록 설정한 경우 `True`이고, 그렇지 않으면 `False`입니다.
* `scoring_uri` - REST API 주소입니다.

배포된 웹 서비스에 대해 이 정보를 검색하는 세 가지 방법이 있습니다.

* 모델을 배포하면 `Webservice` 개체가 서비스에 대한 정보와 함께 반환됩니다.

    ```python
    service = Webservice.deploy_from_model(name='myservice',
                                           deployment_config=myconfig,
                                           models=[model],
                                           image_config=image_config,
                                           workspace=ws)
    print(service.scoring_uri)
    ```

* `Webservice.list`를 사용하여 작업 영역의 모델에 대해 배포된 웹 서비스 목록을 검색할 수 있습니다. 필터를 추가하여 반환되는 정보의 목록을 좁힐 수 있습니다. 필터링할 수 있는 항목에 대한 자세한 내용은 [Webservice.list](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py#list) 참조 설명서를 참조하세요.

    ```python
    services = Webservice.list(ws)
    print(services[0].scoring_uri)
    ```

* 배포된 서비스의 이름을 알고 있으면 `Webservice`의 새 인스턴스 만들고 작업 영역 및 서비스 이름을 매개 변수로 제공할 수 있습니다. 새 개체에는 배포된 서비스에 대한 정보가 포함되어 있습니다.

    ```python
    service = Webservice(workspace=ws, name='myservice')
    print(service.scoring_uri)
    ```

### <a name="authentication-key"></a>인증 키

인증 키는 배포에 대해 인증이 활성화될 때 자동으로 생성됩니다.

* __Azure Kubernetes Service__에 배포할 때 인증은 __기본적으로 활성화__되어 있습니다.
* __Azure 컨테이너 인스턴스__에 배포할 때 인증은 __기본적으로 비활성화__되어 있습니다.

인증을 제어하려면 배포를 만들거나 업데이트할 때 `auth_enabled` 매개 변수를 사용합니다.

인증을 사용하도록 설정한 경우 `get_keys` 메서드를 사용하여 기본 및 보조 인증 키를 검색할 수 있습니다.

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> 키를 다시 생성해야 하는 경우 [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#regen-key)를 사용합니다.

## <a name="request-data"></a>요청 데이터

REST API는 요청 본문이 다음과 같은 구조의 JSON 문서가 될 것으로 예상합니다.

```json
{
    "data":
        [
            <model-specific-data-structure>
        ]
}
```

> [!IMPORTANT]
> 데이터의 구조는 서비스의 채점 스크립트 및 모델이 예상하는 것과 일치해야 합니다. 채점 스크립트는 모델에 데이터를 전달하기 전에 데이터를 수정할 수 있습니다.

예를 들어 [노트북 내에서 학습](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) 예제의 모델에는 10개의 숫자 배열이 필요합니다. 이 예의 채점 스크립트는 요청에서 Numpy 배열을 만들고 모델에 전달합니다. 다음 예제에서는 이 서비스에서 필요한 데이터를 보여줍니다.

```json
{
    "data": 
        [
            [
                0.0199132141783263, 
                0.0506801187398187, 
                0.104808689473925, 
                0.0700725447072635, 
                -0.0359677812752396, 
                -0.0266789028311707, 
                -0.0249926566315915, 
                -0.00259226199818282, 
                0.00371173823343597, 
                0.0403433716478807
            ]
        ]
}
``` 

웹 서비스는 한 번의 요청으로 여러 데이터 세트를 수락할 수 있습니다. 응답 배열을 포함하는 JSON 문서를 반환합니다.

## <a name="call-the-service-c"></a>서비스 호출(C#)

이 예제에서는 C#을 사용하여 [노트북 내에서 학습](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) 예제에서 생성된 웹 서비스를 호출하는 방법을 보여줍니다.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MLWebServiceClient
{
    // The data structure expected by the service
    internal class InputData
    {
        [JsonProperty("data")]
        // The service used by this example expects an array containing
        //   one or more arrays of doubles
        internal double[,] data;
    }
    class Program
    {
        static void Main(string[] args)
        {
            // Set the scoring URI and authentication key
            string scoringUri = "<your web service URI>";
            string authKey = "<your key>";

            // Set the data to be sent to the service.
            // In this case, we are sending two sets of data to be scored.
            InputData payload = new InputData();
            payload.data = new double[,] {
                {
                    0.0199132141783263,
                    0.0506801187398187,
                    0.104808689473925,
                    0.0700725447072635,
                    -0.0359677812752396,
                    -0.0266789028311707,
                    -0.0249926566315915,
                    -0.00259226199818282,
                    0.00371173823343597,
                    0.0403433716478807
                },
                {
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303
                }
            };

            // Create the HTTP client
            HttpClient client = new HttpClient();
            // Set the auth header. Only needed if the web service requires authentication.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", authKey);

            // Make the request
            try {
                var request = new HttpRequestMessage(HttpMethod.Post, new Uri(scoringUri));
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;
                // Display the response from the web service
                Console.WriteLine(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
    }
}
```

반환되는 결과는 JSON 문서와 유사합니다.

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-go"></a>서비스 호출(Go)

이 예제에서는 Go를 사용하여 [노트북 내에서 학습](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) 예제에서 생성된 웹 서비스를 호출하는 방법을 보여줍니다.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// Features for this model are an array of decimal values
type Features []float64

// The web service input can accept multiple sets of values for scoring
type InputData struct {
    Data []Features `json:"data",omitempty`
}

// Define some example data
var exampleData = []Features{
    []float64{
        0.0199132141783263, 
        0.0506801187398187, 
        0.104808689473925, 
        0.0700725447072635, 
        -0.0359677812752396, 
        -0.0266789028311707, 
        -0.0249926566315915, 
        -0.00259226199818282, 
        0.00371173823343597, 
        0.0403433716478807,
    },
    []float64{
        -0.0127796318808497, 
        -0.044641636506989, 
        0.0606183944448076, 
        0.0528581912385822, 
        0.0479653430750293, 
        0.0293746718291555, 
        -0.0176293810234174, 
        0.0343088588777263, 
        0.0702112981933102, 
        0.00720651632920303,
    },
}

// Set to the URI for your service
var serviceUri string = "<your web service URI>"
// Set to the authentication key (if any) for your service
var authKey string = "<your key>"

func main() {
    // Create the input data from example data
    jsonData := InputData{
        Data: exampleData,
    }
    // Create JSON from it and create the body for the HTTP request
    jsonValue, _ := json.Marshal(jsonData)
    body := bytes.NewBuffer(jsonValue)

    // Create the HTTP request
    client := &http.Client{}
    request, err := http.NewRequest("POST", serviceUri, body)
    request.Header.Add("Content-Type", "application/json")

    // These next two are only needed if using an authentication key
    bearer := fmt.Sprintf("Bearer %v", authKey)
    request.Header.Add("Authorization", bearer)

    // Send the request to the web service
    resp, err := client.Do(request)
    if err != nil {
        fmt.Println("Failure: ", err)
    }

    // Display the response received
    respBody, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(respBody))
}
```

반환되는 결과는 JSON 문서와 유사합니다.

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-java"></a>서비스 호출(Java)

이 예제에서는 Java를 사용하여 [노트북 내에서 학습](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) 예제에서 생성된 웹 서비스를 호출하는 방법을 보여줍니다.

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;
import org.apache.http.entity.ContentType;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

public class App {
    // Handle making the request
    public static void sendRequest(String data) {
        // Replace with the scoring_uri of your service
        String uri = "<your web service URI>";
        // If using authentication, replace with the auth key
        String key = "<your key>";
        try {
            // Create the request
            Content content = Request.Post(uri)
            .addHeader("Content-Type", "application/json")
            // Only needed if using authentication
            .addHeader("Authorization", "Bearer " + key)
            // Set the JSON data as the body
            .bodyString(data, ContentType.APPLICATION_JSON)
            // Make the request and display the response.
            .execute().returnContent();
            System.out.println(content);
        }
        catch (IOException e) {
            System.out.println(e);
        }
    }
    public static void main(String[] args) {
        // Create the data to send to the service
        JSONObject obj = new JSONObject();
        // In this case, it's an array of arrays
        JSONArray dataItems = new JSONArray();
        // Inner array has 10 elements
        JSONArray item1 = new JSONArray();
        item1.add(0.0199132141783263);
        item1.add(0.0506801187398187);
        item1.add(0.104808689473925);
        item1.add(0.0700725447072635);
        item1.add(-0.0359677812752396);
        item1.add(-0.0266789028311707);
        item1.add(-0.0249926566315915);
        item1.add(-0.00259226199818282);
        item1.add(0.00371173823343597);
        item1.add(0.0403433716478807);
        // Add the first set of data to be scored
        dataItems.add(item1);
        // Create and add the second set
        JSONArray item2 = new JSONArray();
        item2.add(-0.0127796318808497);
        item2.add(-0.044641636506989);
        item2.add(0.0606183944448076);
        item2.add(0.0528581912385822);
        item2.add(0.0479653430750293);
        item2.add(0.0293746718291555);
        item2.add(-0.0176293810234174);
        item2.add(0.0343088588777263);
        item2.add(0.0702112981933102);
        item2.add(0.00720651632920303);
        dataItems.add(item2);
        obj.put("data", dataItems);

        // Make the request using the JSON document string
        sendRequest(obj.toJSONString());
    }
}
```

반환되는 결과는 JSON 문서와 유사합니다.

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-python"></a>서비스 호출(Python)

이 예제에서는 Python을 사용하여 [노트북 내에서 학습](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) 예제에서 생성된 웹 서비스를 호출하는 방법을 보여줍니다.

```python
import requests
import requests
import json

# URL for the web service
scoring_uri = '<your web service URI>'
# If the service is authenticated, set the key
key = '<your key>'

# Two sets of data to score, so we get two results back
data = {"data": 
            [
                [
                    0.0199132141783263, 
                    0.0506801187398187, 
                    0.104808689473925, 
                    0.0700725447072635, 
                    -0.0359677812752396, 
                    -0.0266789028311707, 
                    -0.0249926566315915, 
                    -0.00259226199818282, 
                    0.00371173823343597, 
                    0.0403433716478807
                ],
                [
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303]
            ]
        }
# Convert to JSON string
input_data = json.dumps(data)

# Set the content type
headers = { 'Content-Type':'application/json' }
# If authentication is enabled, set the authorization header
headers['Authorization']=f'Bearer {key}'

# Make the request and display the response
resp = requests.post(scoring_uri, input_data, headers = headers)
print(resp.text)

```

반환되는 결과는 JSON 문서와 유사합니다.

```JSON
[217.67978776218715, 224.78937091757172]
```

## <a name="next-steps"></a>다음 단계

이제 배포된 모델에 대한 클라이언트를 만드는 방법을 배웠으므로, [IoT Edge 디바이스에 모델을 배포](how-to-deploy-to-iot.md)하는 방법을 알아봅니다.
---
title: Azure Data Factory Mapping Data Flow 만들기
description: Azure Data Factory Mapping Data Flow 만들기
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: b706e229bed48c821d5ca772450df320fd7e0b7f
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271395"
---
# <a name="create-azure-data-factory-data-flow"></a>Azure Data Factory Data Flow 만들기

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

ADF의 Mapping Data Flow는 코딩하지 않고도 대규모 데이터를 변환할 수 있는 방법을 제공합니다. 일련의 변환을 생성하여 데이터 흐름 디자이너에서 데이터 변환 작업을 설계할 수 있습니다. 임의 개수의 원본 변환으로 시작한 다음, 데이터 변환 단계를 수행합니다. 데이터 흐름을 싱크로 완료하여 결과를 대상으로 이동합니다.

먼저 Azure Portal에서 새 V2 Data Factory를 만듭니다. 새 팩터리를 만든 후 “작성자 및 모니터” 타일을 클릭하여 Data Factory UI를 시작합니다.

![데이터 흐름 옵션](media/data-flow/v2dataflowportal.png "데이터 흐름 만들기")

Data Factory UI가 표시되면 샘플 데이터 흐름을 사용할 수 있습니다. 이 샘플은 ADF 템플릿 갤러리에서 제공됩니다. ADF에서 “템플릿에서 파이프라인”을 만들고 템플릿 갤러리에서 데이터 흐름 범주를 선택합니다.

![데이터 흐름 옵션](media/data-flow/template.png "데이터 흐름 만들기")

Azure Blob Storage 계정 정보를 입력하라는 메시지가 표시됩니다.

![데이터 흐름 옵션](media/data-flow/template2.png "데이터 흐름 만들기 2")

[이 샘플에 사용되는 데이터는 여기서 확인할 수 있습니다](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). 샘플을 실행할 수 있도록 샘플 데이터를 다운로드하고 Azure Blob Storage 계정에 파일을 저장합니다.

ADF UI의 리소스 만들기 “더하기” 단추를 사용하여 데이터 흐름 만들기

![데이터 흐름 옵션](media/data-flow/newresource.png "새 리소스")

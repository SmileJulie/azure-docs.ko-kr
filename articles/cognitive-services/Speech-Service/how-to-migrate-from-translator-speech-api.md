---
title: Translator Speech API에서 Speech Service로 마이그레이션
titleSuffix: Azure Cognitive Services
description: 이 항목을 사용하여 Translator Speech API에서 Speech Service로 응용 프로그램을 마이그레이션합니다.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: aahi
ms.openlocfilehash: a35c4a7f2d2b64bd3650df1f18d08da4ea191e3a
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466395"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Translator Speech API에서 Speech Service로 마이그레이션

이 문서를 사용하여 Microsoft Translator Speech API에서 [Speech Service](index.yml)로 응용 프로그램을 마이그레이션합니다. 이 가이드에서는 Translator Speech API와 Speech Service 간의 차이점을 간략하게 설명하고 응용 프로그램을 마이그레이션하기 위한 전략을 제안합니다.

> [!NOTE]
> Translator Speech API 구독 키는 Speech Service에서 허용되지 않습니다. 새 Speech Service 구독을 시작해야 합니다.

## <a name="comparison-of-features"></a>기능 비교

| 기능                                           | Translator Speech API                                  | Speech Service | 세부 정보                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 텍스트로 번역                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| 음성으로 번역                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| 전역 엔드포인트                                   | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Speech Service는 현재 전역 엔드포인트를 제공하지 않습니다. 전역 엔드포인트는 트래픽을 가장 가까운 지역 엔드포인트로 자동으로 전송하여 응용 프로그램의 대기 시간을 줄일 수 있습니다.                                                    |
| 지역별 엔드포인트                                | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| 연결 시간 제한                             | 90분                                               | SDK를 사용할 경우 무제한 WebSocket 연결을 사용할 경우 10분                                                                                                                                                                                                                                                                                   |
| 헤더의 인증 키                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| 단일 요청을 통해 여러 언어 번역 | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| SDK 사용 가능                                    | :heavy_minus_sign:                                              | :heavy_check_mark:                 | 사용 가능한 SDK에 대해서는 [Speech Service 설명서](index.yml)를 참조하세요.                                                                                                                                                    |
| WebSocket 연결                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Languages API                                     | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Speech Service는 [Translator API 언어 참조](../translator-speech/languages-reference.md) 문서에 설명된 것과 동일한 언어 범위를 지원합니다. |
| 욕설 필터 및 표식                       | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| .WAV/PCM을 입력으로 사용                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| 다른 파일 형식을 입력으로 사용                         | :heavy_minus_sign:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| 부분 결과                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| 타이밍 정보                                       | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                 |
| 상관관계 ID                                    | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Custom Speech 모델                              | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Speech Service는 음성 인식을 조직의 고유한 어휘로 사용자 지정할 수 있는 Custom Speech 모델을 제공합니다.                                                                                                                                           |
| 사용자 지정 번역 모델                         | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Microsoft Text Translation API를 구독하면 [Custom Translator](https://www.microsoft.com/translator/business/customization/)(현재 미리 보기로 제공)를 사용하여 보다 정확한 번역을 위해 자체 데이터를 사용할 수 있습니다.                                                 |

## <a name="migration-strategies"></a>마이그레이션 전략

사용자 또는 조직의 개발 또는 프로덕션 환경에 Translator Speech API를 사용하는 응용 프로그램이 있는 경우 Speech Service를 사용하도록 업데이트해야 합니다. 사용 가능한 SDK, 코드 샘플 및 자습서에 대해서는 [Speech Service](index.yml) 설명서를 참조하세요. 마이그레이션 시 고려할 사항은 다음과 같습니다.

* Speech Service는 현재 전역 엔드포인트를 제공하지 않습니다. 응용 프로그램모든 트래픽에 대해 단일 지역별 엔드포인트를 사용하여 효율적으로 작동하는지를 확인해야 합니다. 그렇지 않을 경우 지리적 위치를 사용하여 가장 효율적인 엔드포인트를 확인해야 합니다.

* 응용 프로그램 수명이 긴 연결을 사용하며 사용 가능한 SDK를 사용할 수 없는 경우 WebSocket 연결을 사용하고, 적절한 시간에 다시 연결하여 10분 간의 시간 제한을 관리할 수 있습니다.

* 응용 프로그램이 Translator Text API 및 Translator Speech API를 사용하여 사용자 지정 번역 모델을 사용하도록 설정하는 경우 Speech Service를 사용하여 ‘범주’ ID를 직접 추가할 수 있습니다.

* Speech Service는 Translator Speech API와 달리 단일 요청을 통해 여러 언어로 완전히 번역할 수 있습니다.

## <a name="next-steps"></a>다음 단계

* [Speech Service 체험해 보기](get-started.md)
* [빠른 시작: Speech SDK를 사용하여 UWP 앱에서 음성 인식](quickstart-csharp-uwp.md)

## <a name="see-also"></a>참고 항목

* [Speech Service란?](overview.md)
* [Speech Service 및 SDK 설명서](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
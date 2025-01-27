---
title: 음향 시뮬레이션을 사용한 디자인 개념
titlesuffix: Azure Cognitive Services
description: 이 개념 개요 프로젝트 소음 음향 시뮬레이션을 적절 하 게 디자인 프로세스를 통합 하는 방법을 설명 합니다.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 4a1a0b15da091a1c020eb132f6b14b9ee14d334c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61335420"
---
# <a name="project-acoustics-design-process-concepts"></a>프로젝트 소음 디자인 프로세스 개념

이 개념 개요 프로젝트 소음 적절 하 게 디자인 프로세스에 실제 음향 시뮬레이션을 통합 하는 방법을 설명 합니다.

## <a name="sound-design-with-audio-dsp-parameters"></a>오디오 DSP 매개 변수를 사용 하 여 적절 하 게 디자인

대화형 3D 제목 오디오 엔진에서 호스트 되는 (DSP) 블록을 처리 하는 오디오 디지털 신호를 사용 하 여 해당 특정 소리를 달성 합니다. 이러한 블록 반향, 에코, 지연, 균등화, 압축 및 제한, 간단한 혼합 및 기타 효과 이르기까지 다양 합니다. 선택 하면, 정렬 및 이러한 효과의 매개 변수 설정 환경의 미학 및 게임 플레이 목표를 달성 하는 오디오 그래프를 작성 하는 사운드 디자이너의 담당 합니다.

대화형 제목에 소리 및 수신기를 3D 공간 전체에 걸쳐 이동 하는 방법을 수행 이러한 매개 변수 변화에 적응할 조건? 사운드 디자이너 종종 전반에 대 한 매개 변수 변경을 수신기 장면에의 한 부분에서 다른 전환 예를 들어, 또는 조합에 오리 소리 반향 효과의 변경 내용을 달성 하기 위해 트리거할 프로그래밍 된 볼륨으로 정렬 됩니다. 소음 시스템도 사용할 수 있는 자동화할 수 있는 이러한 효과 중 일부입니다.

3D 제목 물리학 동기 부여 되지만 다양 한 집중 교육 및 게임 플레이 목표를 달성 하기 위해 디자이너 조정 된 조명 및 운동학 적 물리학 시스템을 사용 합니다. 비주얼 디자이너를 개별 픽셀 값을 설정 하지 않습니다 하지만 대신 3D 모델, 자료 및 밝은 전송 시스템 모든 물리적 기반 visual 미관 및 CPU 비용의 균형을 조정 합니다. 해당 하는 프로세스 오디오에 대 한 것이 무엇 인지? 프로젝트 소음에 탐색이 질문을 첫 번째 단계입니다. 먼저 공간을 통해 음향 에너지를 전송 하는 것에 touch 됩니다 것입니다.

![스크린 샷의 AltSpace 장면으로 반향 영역 중첩](media/reverb-zones-altspace.png)

## <a name="impulse-responses-acoustically-connecting-two-points-in-space"></a>임펄스 응답: 공간에서 두 점 acoustically 연결

오디오 디자인에 익숙한 경우 음향 임펄스 응답을 사용 하 여 친숙 한 수도 있습니다. Acoustic 임펄스 응답을 모델링 원본에서 소리를 수신기에 전송 합니다. 따라서 임펄스 응답을 반향 폐색 등 방 소음의 모든 흥미로운 효과 캡처할 수 있습니다. 임펄스 응답 오디오 DSP 효과를 조정할 수 있도록 특정 강력한 속성도 포함 되어 있습니다. 두 개의 오디오 신호를 함께 추가 및 임펄스 응답을 처리 하는 경우 각 신호를 별도로 임펄스 응답을 적용 하 고 결과 추가 하는 것과 동일한 결과 제공 합니다. Acoustic 전파 및 임펄스 응답도에 의존 하지 모델링 되 고 장면에만 처리 중인 오디오 및 소스와 수신기 위치 합니다. 즉, 임펄스 응답을 사운드 전파에 장면의 효과 추출 합니다.

임펄스 응답을 모든 흥미로운 방 음향 효과 및 적용할 수도 있습니다 오디오를 효율적으로 필터를 사용 하 여 캡처하고 임펄스 응답 측정 또는 시뮬레이션에서 가져올 수 있습니다. 하지만 경우에 어떻게에서는 없는 매우 물리를 정확 하 게 일치 하도록 소음 싶지만 대신 장면의 감정적인 요구에 맞게 조정할? 하지만 훨씬 픽셀 값과 같이 임펄스 응답을 수천 개의 숫자 목록 일 뿐 이면 어떻게 가능한 조정할 수 미적 요구를 충족 하도록? 및 폐색/드롭을 달라 지는 원활 하 게 빠르거나의 입구를 통해 전달 하는 동안 얼마나 많은 임펄스 응답 해야 부드러운 효과 하고자 될까요? 그렇다면 원본 빠른 이동? 어떻게 보간에서는?

대화형 제목에 소음의 일부 측면에 대 한 시뮬레이션 및 임펄스 응답을 사용 하기 어려운 보인다는 것입니다. 하지만 대 한 임펄스 응답 시뮬레이션에서 친숙 한 오디오 DSP 효과 매개 변수를 사용 하 여 연결할 수 경우 디자이너 조정을 지 원하는 오디오 전송 시스템을 여전히 구축할 수 있습니다.

## <a name="connecting-simulation-to-audio-dsp-with-parameters"></a>시뮬레이션 매개 변수를 사용 하 여 오디오 DSP에 연결

모든 흥미로운 임펄스 응답을 포함 (매 필요 하지 않은) 음향 효과입니다. 오디오 DSP 블록, 해당 매개 변수가 올바르게 설정 하는 경우 흥미로운 음향 효과 렌더링할 수 있습니다. 음향 시뮬레이션을 사용 하 여 3D 장면에 오디오 전송을 자동화 하는 오디오 DSP 블록 드라이브를 오디오 DSP 매개 변수를 임펄스 응답을 측정 하는 것입니다. 이 측정 폐색, 드롭을, portalling을 반향 등 특정 일반 및 중요 음향 효과 대 한 잘 알려져 있습니다.

하지만 디자이너 조정 되는 시뮬레이션 오디오 DSP 매개 변수를 직접 연결 되어 있습니까? 새로운 않았습니다 이점도? 물론 임펄스 응답을 삭제 하 고 몇 가지 DSP 매개 변수를 저장 함으로써 다시 상당한 양의 메모리를 얻으세요. 및에 부여 하려면 디자이너 어느 정도의 최종 결과 대해 필요한만 알게 시뮬레이션 사이의 오디오 DSP 디자이너를 삽입 하는 방법을 합니다.

![오버레이 하는 매개 변수를 사용 하 여 스타일이 적용 된 임펄스 응답을 사용 하 여 그래프](media/acoustic-parameters.png)

## <a name="sound-design-by-transforming-audio-dsp-parameters-from-simulation"></a>시뮬레이션에서 오디오 DSP 매개 변수를 변환 하 여 적절 하 게 디자인

전 세계의 보기 선글라스에 미칠 영향을 고려해 야 합니다. 밝은 하루에는 안경을 보다 편리 하 게 되는 빛을 줄일 수 있습니다. 수 어두운 방에는 아무 것도 전혀 볼 수 없습니다. 안경; 모든 상황에서 특정 수준의 밝기를 설정 하지 낮아질 음영이 짙을 수록 모든 항목입니다.

시뮬레이션 폐색 및 반향 매개 변수를 사용 하 여이 오디오 DSP 드라이브를 사용 하는 경우 시뮬레이터를 DSP '확인' 매개 변수를 조정 후 필터를 추가할 수 있습니다. 필터 일정 수준의 폐색 적용 또는 선글라스 없는 확보 마다 동일한 밝기 같은 훨씬 비상 길이 잔 향 하지 않습니다. 필터 마다 occluder 작은 채워집니다 확인 될 수 있습니다. 또는 자세히 채워집니다. 를 추가 하 고 하나의 '어둡게' 폐색 매개 변수 필터를 조정 하 여 대규모의 열기 방은 아직 거의 영향을 주지 폐색, 입구 늘어나기 매체에서 강력한 폐색 효과를 적용 부드러운 정도 유지 하면서 전환 하는 동안 시뮬레이션을 제공 합니다.

이 패러다임은 디자이너의 작업 선택에 선택 및 조정에 적용할 필터를 시뮬레이션에서 가져온 가장 중요 한 DSP 매개 변수를 각 상황에 대 한 어쿠스틱 매개에서 변경 됩니다. 이 높은 문제를 부드럽게 전환 폐색 및 반향 효과의 강도 및 원본 조합에 있는지 여부를 설정 하는 좁은 문제에서 디자이너의 활동을 승격 시킵니다. 물론 상황을 요청 하는 경우 항상 사용 가능한 필터 중 하나의 단순히 다시 이동 상황에 따라 특정 원본에 대 한 DSP 매개 변수를 선택 하입니다.

## <a name="sound-design-in-project-acoustics"></a>프로젝트 소음에 적절 하 게 디자인

각 위에서 설명한 구성 요소 프로젝트 소음 패키지 통합: 시뮬레이터, 매개 변수를 추출 하 고 소음 자산, 오디오 DSP 및 다양 한 필터를 작성 하는 인코더입니다. 프로젝트 소음을 사용 하 여 적절 하 게 디자인 폐색 및 반향 매개 변수를 조정 시뮬레이션에서 파생 되며 게임 편집기 및 오디오 엔진 내에서 노출 하는 동적 컨트롤을 사용 하 여 오디오 DSP에 적용 된 필터에 대 한 선택 매개 변수를 수반 합니다.

## <a name="next-steps"></a>다음 단계
* 사용 하 여 디자인 패러다임 사용해 합니다 [Unity에 대 한 빠른 시작 프로젝트 소음](unity-quickstart.md) 또는 [Unreal 프로젝트 소음 빠른 시작](unreal-quickstart.md)
* 탐색 합니다 [Unity에 대 한 컨트롤을 디자인 하는 프로젝트 소음](unity-workflow.md) 또는 [프로젝트 소음 Unreal에 대 한 컨트롤을 디자인](unreal-workflow.md)


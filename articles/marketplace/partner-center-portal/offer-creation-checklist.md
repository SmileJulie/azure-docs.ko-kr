---
title: Azure Marketplace 상용 작성 검사 목록-제공
description: 세부 정보에서 제품 만들기 프로세스를 제공할 수 있습니다. -Azure Marketplace 고급
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: eb824eb67e84ec4bdb93bc355ac6a6afa844ceb9
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67701164"
---
# <a name="offer-creation-checklist"></a>제안 만들기 검사 목록

제품 만들기 프로세스를 여러 페이지를 통해 이동 됩니다. 각 항목에 대 한 자세한 내용을 보려면 링크를 사용 하 여 각 페이지에 제공할 수 있습니다 세부 정보는 다음과 같습니다.

제공 하거나 지정 해야 하는 항목 아래에 나와 있습니다. 일부 영역은 선택적 또는 기본 값 제공을 원하는 대로 변경할 수 있습니다. 여기에 나열 된 순서 대로 이러한 섹션에 사용할 필요가 없습니다.

| **항목**    | **용도**  |
| :---------- | :-------------------|
| [**새 제품 모달**](#new-offer-modal) | 수집 id 정보를 제공합니다.  |
| [제품 설치 페이지](#offer-setup-page) | 주요 기능을 사용 하 여 Microsoft 통해 제품을 판매 하는 방법을 선택에서 선택할 수 있습니다.  |
| [속성 페이지](#properties-page) | 마켓플레이스, 제품, 및 앱 버전을 지 원하는 법적 계약에 대 한 제품을 그룹화 하는 데 사용 되는 산업 고 범주를 정의 합니다. |
| [제품 목록 페이지](#offer-listing-page) | 마케팅 자산 및 제품 설명을 포함 하 여 marketplace에서 표시할 제안 세부 정보를 정의 합니다. |
| [미리 보기 페이지](#preview-page) | 광범위 한 marketplace 사용자를 라이브 제품을 게시 하기 전에 제품 출시에 대 한 제한 된 미리 보기 사용자를 정의 합니다. |
| [기술 구성 페이지를 제공 합니다.](#technical-configuration-page)  | Microsoft 통해 제품을 판매 하도록 선택한 경우에 사용할 수 있습니다. 제품에 연결할 때 사용할 기술 정보 (예: URL 경로, webhook, 테 넌 트 ID 및 앱 ID)를 정의 합니다. |
| [**새 계획 모달**](#plan-identity-modal) | 수집 계획 id 정보입니다.  |
| [계획 목록 페이지](#plan-listing-page)  | Microsoft 통해 제품을 판매 하도록 선택한 경우에 사용할 수 있습니다. Marketplace에서 계획을 나열 하는 데 사용 하는 세부 정보를 정의 합니다.  |
| [계획 가격 책정 및 가용성 페이지](#plan-pricing--availability-page)  | Microsoft 통해 제품을 판매 하도록 선택한 경우에 사용할 수 있습니다.  제품의 각 계획 (버전)에 대 한 대상 그룹 및 시장 가용성을 비즈니스 특성 (가격 책정 모델)을 수집 합니다.  |
| [테스트 드라이브 목록 페이지](#test-drive-listing-page)  | 제품에 대 한 시험 사용을 제공 하도록 선택한 경우에 사용할 수 있습니다. Marketplace에서 테스트 드라이브 목록에 사용 되는 세부 정보를 정의 합니다.  |
| 테스트 드라이브 기술 구성 페이지  | 제품에 대 한 시험 사용을 제공 하도록 선택한 경우에 사용할 수 있습니다. 통해 고객은 구매를 커밋하기 전에 제품을 시도 하는 데모 (또는 "시험")에 대 한 기술 세부 정보를 정의 합니다.  |
| [검토 하 고 게시 페이지](#review-and-publish-page)  | 변경 내용을 게시 하 고 각 페이지의 상태를 보려면 인증 팀으로 정보를 제공 하려면 선택 합니다.  |


## <a name="new-offer-modal"></a>모달 새 제품 

첫 번째 정보 부분으로 제공 하 라는 메시지가 표시 됩니다는 ID 및 제품에 대 한 별칭입니다. 

| **필드 이름**    | **참고**   |  
| :---------------- | :-----------| 
| 제품 ID  | 필수, 만든 후 변경할 수 없습니다. 최대 50 자 및 소문자 영숫자 문자, 대시 또는 밑줄만 구성 되어야 합니다. |
| 제품 별칭  | 필수 요소. |

## <a name="offer-setup-page"></a>제품 설치 페이지

제품 설치 페이지는 여기서 있습니다 수 다른 채널 및 판매 동작을 옵트인 시험 등 고객의 주요 기능을 사용 하면 선언 합니다. 

| **필드 이름**    | **참고**   | 
| :---------------- | :-----------|  
| Microsoft를 통해 판매 하 시겠습니까?  | 필수 요소. 기본값: 예 |
| 어떻게 시겠습니까 제품과 상호 작용 하는 잠재 고객 목록? (작업 호출)  | Microsoft를 통해 판매 되지 경우 필요 합니다. 기본값: 무료 평가판 옵션: "지금 가져오기", "체험 평가판", "나에 게 연락 합니다." |
| 체험 URL  | 제품 목록을 상호 작용 하는 방식으로 고객은 "체험 평가판" 선택 된 경우 필요 합니다. |
| 혜택 URL  | 제품 목록 상호 작용 하는 방식으로 고객 "이 지금" 선택 된 경우 필요 |
| 채널  | 선택 사항입니다. 기본값: CSP (대리점) 채널에 가입 되지 합니다.  |
| 시험 사용 | 선택 사항입니다. 기본값: 테스트 드라이브를 사용할 수 없습니다.  |
| 시험 사용 유형 | 시험 사용 하도록 설정 하는 경우 필요 합니다. 기본값: 선택 하지 않았습니다. 옵션: Azure Resource Manager, Dynamics 365 Business Central에 대 한 Dynamics 365 Customer Engagement, 작업, 논리 앱에 대 한 Dynamics 365에 대 한 Power BI입니다.  |
| 잠재 고객 관리-CRM 시스템에 연결 | Microsoft를 통해 판매 또는 "나에 게 연락 합니다." 대로 목록을 제공 하는 경우 필요 합니다. 기본값: CRM 시스템 연결 합니다. CRM 옵션: Azure 테이블, Azure blob, Dynamics CRM online, HTTPs의 끝점, Marketo, Salesforce  |

## <a name="properties-page"></a>속성 페이지

속성 페이지가 범주 및 마켓플레이스를, 제품 및 앱 버전을 지 원하는 법적 계약에 대 한 제품을 그룹화 하는 데 사용 되는 산업을 정의 하는 위치입니다. 적절 하 게 표시 되 고 고객의 올바른 집합을 제공 하는 수 있도록이 페이지에서는 제품에 대 한 완전 하 고 정확한 정보를 제공 해야 합니다. 

| **필드 이름**    | **참고**   | 
| :---------------- | :-----------|  
| 범주 및 하위 범주 | 1 자에서 최대 3이 필요합니다. 기본값: 선택 하지 않았습니다. |
| 산업 및 subindustries | 선택 사항입니다. 최대 2 L1 산업 및 기본 각 L1 업계 내에서 2 subindustries 최대: 선택된 항목 없음 |
| 앱 버전  | 선택 사항입니다. 기본값: 없음 |
| 표준 계약 사용  | 선택 사항입니다. 기본값: 선택 하지 않습니다.  | |
| 사용 약관  | 표준 계약 선택 하지 않은 경우 필요 합니다.  |

## <a name="offer-listing-page"></a>제품 목록 페이지

목록 페이지는 고객이 marketplace에 제품의 목록을 볼 때 표시 되는 이미지와 텍스트를 제공 하는 위치입니다. 

| **필드 이름**    | **참고**   |
| :---------------- | :-----------| 
| 이름  | 필요한 경우 최대 50 자입니다. |
| 요약  | 필요한 경우 최대 100 자입니다. | 
| 설명  | 필요한 경우 최대 3000 자입니다. |
| 시작하기 지침  | 필요한 경우 최대 3000 자입니다. |
| 시작하기 지침  | 필요한 경우 최대 3000 자입니다. |
| 검색 키워드  | 선택 사항, 권장 되는 최대 3 가지 키워드입니다. |
| 개인정보 처리 방침 URL  | 필수 요소. |
| CSP 프로그램 마케팅 자료 URL  | 선택 사항입니다. |
| 유용한 링크 제목 + URL  | 선택 사항입니다. |
| 제목 + 파일 문서 지원  | 필수, 최소 1 및 3 max입니다. PDF 파일 형식 이어야 합니다. |
| 스크린샷  | 필수, 최소 1 스크린샷 및 최대 5; 4 이상의 것이 좋습니다. 1280 X 720에 PNG 형식 이어야 합니다. |
| 로고를 저장 (소규모, 중간, 큼, 차원 Hero)  | 소형 (48 X 48) 및 대형 (216x216) 숫자 식입니다. 선택 사항 이지만 권장 되는 다른 크기: 보통 (90 x 90), 와이드 (255 x 115), Hero (815 x 290)입니다. PNG 형식 이어야 합니다. |
| 비디오 이름 + URL 미리 보기  | 선택 사항, 권장 되는 최대 4 개의 비디오. 미리 보기에는 1280 x 720 PNG 형식에서 이어야 합니다. YouTube 또는 Vimeo 비디오를 호스팅해야 합니다. |
| 연락처 (CSP 프로그램, 엔지니어링, 지원)  | 엔지니어링 및 지원 담당자 (이름, 전자 메일 및 전화 번호) 숫자 식입니다. 선택 사항 이지만 권장 되는 CSP 프로그램 연락처입니다. |
| 지원 URL  | 필수 요소. |

## <a name="preview-page"></a>미리 보기 페이지

미리 보기 페이지는 제품이 라이브 전에 모든 요구 사항을 충족 하는지 확인 하려면 제품 미리 보기에 액세스할 수 있도록 대상을 지정 하 합니다. 

| **필드 이름**    | **참고**   | 
| :---------------- | :-----------| 
| AAD/MSA 전자 메일 + 설명 | 최소 1 및 CSV 파일을 업로드 하는 경우 수동으로 또는 20까지 입력 한 경우 최대 10 필요 합니다. |

## <a name="technical-configuration-page"></a>기술 구성 페이지 

기술 구성 페이지가 제품에 연결할 Microsoft에서 사용 하는 기술 세부 정보를 지정 하는 위치입니다. Microsoft를 통해 판매 하지 않기로 결정 하는 경우에이 페이지에 표시 되지 않습니다.

| **필드 이름**    | **참고**   |  
| :---------------- | :-----------| 
| 방문 페이지 URL | Microsoft를 통해 판매 하는 경우 필요 합니다. |
| 연결 웹 후크 | Microsoft를 통해 판매 하는 경우 필요 합니다. |
| Azure AD 테넌트 ID | Microsoft를 통해 판매 하는 경우 필요 합니다. |
| Azure AD 앱 ID | Microsoft를 통해 판매 하는 경우 필요 합니다. |

## <a name="plan-identity-modal"></a>Identity 모달 계획

첫 번째 부분 정보를 제공 하는 메시지가 이름 및 계획에 대 한 ID입니다. Microsoft를 통해 판매 필요가 계획인 경우이 페이지에 표시 되지 않습니다.

| **필드 이름**    | **참고**   |  
| :---------------- | :-----------| 
| 계획 ID  | Microsoft를 통해 판매 하는 경우 필요 합니다. 만든 후 변경할 수 없습니다. 최대 50 자 및 소문자 영숫자 문자, 대시 또는 밑줄만 구성 되어야 합니다. |
| 계획 이름입니다.  | Microsoft를 통해 판매 하는 경우 필요 합니다. 고유 해야 제품의 모든 계획 합니다. 최대 50 자입니다. |

## <a name="plan-listing-page"></a>계획 목록 페이지

목록 페이지 계획이 marketplace에서 계획을 볼 때에 고객에 대 한 텍스트를 제공 하는 위치입니다. Microsoft를 통해 판매 하지 않기로 결정 하는 경우에이 페이지에 표시 되지 않습니다.

| **필드 이름**    | **참고**   |  
| :---------------- | :-----------| 
| 계획 설명   | Microsoft를 통해 판매 하는 경우 필요 합니다. 최대 500 자입니다. | |

## <a name="plan-pricing--availability-page"></a>계획 가격 책정 및 가용성 페이지

계획 가격 책정 및 가용성 페이지가 비즈니스 특성, 사용자 및 제품의 각 계획 (버전)에 대 한 시장 가용성을 정의 하는 위치입니다. Microsoft를 통해 판매 하지 않기로 결정 하는 경우에이 페이지에 표시 되지 않습니다.

| **필드 이름**    | **참고**   | 
| :---------------- | :-----------| 
| 시장 가용성  | 필수, 최소 1 및 141 max입니다. |
| 가격 책정 모델  | 필수 요소. 기본값: 요금이 있습니다. 옵션: 사용자 당 플랫 속도입니다. |
| 최소 및 최대 사용자 수  | 선택적이 고, 사용자 기반 가격 책정 모델 선택 하는 경우에 사용할 수 있습니다. |
| 청구 기간  | 필수 요소. 기본값: 월별. 옵션: 월간, 연간 합니다. |
| 가격  | 매월: 매월 선택 하는 용어를 청구 하는 경우 필요한 USD 또는 USD 연간 경우 연간 청구 용어를 선택 합니다. |
| 대상 계획  | 선택 사항입니다. 기본값: 공개 계획 합니다. 옵션: 공용, 사설 테 넌 트 ID로 |
| 제한 계획 대상 (테 넌 트 ID + 설명)  | 개인 계획을 선택한 경우 필요 합니다. 1 분에서 최대 10 테 넌 트 Id를 직접 입력 하는 경우. CSV 파일 가져오기의 최대 20000입니다. |

## <a name="test-drive-listing-page"></a>테스트 드라이브 목록 페이지

제품에 대 한 시험 사용을 제공 하도록 선택한 경우에 사용할 수 있습니다. Marketplace에서 테스트 드라이브 목록에 사용 되는 세부 정보를 정의 합니다.

| **필드 이름**    | **참고**   | 
| :---------------- | :-----------| 
| 설명  | 필수 요소. |
| 수동 사용자 이름 + 파일  | 필요한 경우 최대 1 개 문서입니다. PDF 형식 이어야 합니다. |
| 비디오 이름, URL + 미리 보기  | 선택 사항, 권장 합니다. 축소판 그림 533 x 324 JPGP 또는 PNG 형식 이어야 합니다. YouTube 또는 Vimeo 비디오를 호스팅해야 합니다. |

## <a name="review-and-publish-page"></a>검토 하 고 게시 페이지

| **필드 이름**    | **참고**   | 
| :---------------- | :-----------| 
| 인증에 대 한 정보  | 선택 사항입니다. |

## <a name="next-steps"></a>다음 단계

- [새 SaaS 제품을 만드는](./create-new-saas-offer.md)

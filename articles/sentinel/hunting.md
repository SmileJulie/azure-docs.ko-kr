---
title: Azure Sentinel 미리 보기에서 찾거나 기능 | Microsoft Docs
description: 이 문서에서는 Azure Sentinel hunting 기능을 사용 하는 방법을 설명 합니다.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 6aa9dd27-6506-49c5-8e97-cc1aebecee87
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 23e7db25e5ebed2a23b4d38bcfe9597b77c6b04b
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620736"
---
# <a name="hunt-for-threats-with-in-azure-sentinel-preview"></a>끊임없이 연구 하 고 Azure Sentinel 미리 보기에서 사용 하 여 위협

> [!IMPORTANT]
> Azure Sentinel은 현재 공개 미리 보기로 제공됩니다.
> 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

보안 위협에 대 한 확인 하는 방법에 대 한 문제를 예방 해야 하려는 조사 담당자 라면 검색 및 쿼리 도구를 사용 하 여 조직의 데이터 원본에서 보안 위협에 대 한 hunt를 렵 Azure Sentinel 강력 합니다. 하지만 시스템 및 보안 어플라이언스 생성 방대한 데이터를 구문 분석 하 고 의미 있는 이벤트를 필터링 하기 어려울 수 있습니다. 보안을 위해 분석가 검색할 사전에 Azure Sentinel 보안 앱에 검색 되지 않은 새로운 이상 현상을 ' 네트워크에 이미 있는 데이터의 문제를 찾으려면 올바른 질문 하기에 기본 제공 hunting 쿼리 과정을 안내 합니다. 

예를 들어, 인프라에서 실행 되는 가장 일반적이 지 않은 프로세스에 대 한 데이터를 제공 하는 하나의 기본 제공 쿼리-실행 될 때 각 시간에 대 한 경고를 원하지, 완전히 무고 수 있지만 확인 하는 경우에 따라 쿼리를 확인 하려는 번째 ere 비정상적인 항목의 합니다. 



Azure Sentinel 사냥을 사용 하 여 다음 기능을 수행할 수 있습니다.

- 기본 제공 쿼리: 시작 하기, 시작 페이지를 시작한 테이블 및 쿼리 언어를 사용 하 여 파악 하도록 설계 되었습니다 미리 로드 된 쿼리 예제를 제공 합니다. 새 검색 찾아서으 니 시작 위치를 파악 하는 진입점을 사용 하 여 제공 하기 쿼리 미세 조정 하는 기존 및 새 쿼리 추가 지속적으로, Microsoft 보안 연구원에 의해 개발 된 이러한 기본 제공 hunting 쿼리는 새로운 공격의 처음과 끝입니다. 

- IntelliSense 사용 하 여 강력한 쿼리 언어: Hunting 발전 하는 데 필요한 유연성을 제공 하는 쿼리 언어를 기반으로 합니다.

- 책갈피를 만듭니다. Hunting 과정에서 일치 결과, 대시보드, 질문 또는 비정상적인 또는 의심 스러운 보이는 작업 나올 수 있습니다. 향후에 다시 가입할 수 있도록 해당 항목을 표시를 위해 책갈피 기능을 사용 합니다. 책갈피를 통해 나중에 대 한 조사에 대 한 사례를 만드는 데 사용할 항목을 저장 수 있습니다. 책갈피에 대 한 자세한 내용은 사용 [hunting에서 책갈피]을 참조 하십시오.

- 조사를 자동화 하려면 노트북을 사용 합니다. Notebooks는 조사 단계를 안내 hunt를 빌드할 수 있는 단계별 플레이 북 같습니다.  Notebook 조직의 다른 사용자와 공유할 수 있는 재사용 가능한 플레이 북의 모든 사냥 단계를 캡슐화 합니다. 
- 저장된 된 데이터를 쿼리 합니다. 데이터를 쿼리할 수 있도록 테이블에 액세스할 수 있습니다. 예를 들어 프로세스 만들기, DNS 이벤트 및 다른 많은 이벤트 종류를 쿼리할 수 있습니다.

- 커뮤니티 링크: 추가 쿼리 및 데이터 원본을 찾은 큰 커뮤니티의 힘을 활용 합니다.
 
## <a name="get-started-hunting"></a>렵 시작

1. Sentinel Azure 포털에서 클릭 **사냥**합니다.
  ![Azure Sentinel 시작 구하기](media/tutorial-hunting/hunting-start.png)

2. 여는 경우는 **사냥** 페이지에 모든 hunting 쿼리는 단일 테이블에 표시 됩니다. 테이블 생성 또는 수정 추가 쿼리 뿐만 아니라 보안 분석가 Microsoft 팀에서 작성 한 모든 쿼리를 나열 합니다. 각 쿼리 hunts, 및에서 실행 되 고 어떤 종류의 데이터에 대 한 설명을 제공 합니다. 이러한 템플릿은 해당 다양 한 전략에 따라 그룹화 됩니다-초기 액세스, 지 속성 및 반출 같은 위협 유형을 분류 하는 오른쪽에 있는 아이콘입니다. 필드 중 하나를 사용 하 여 이러한 hunting 쿼리 템플릿을 필터링 할 수 있습니다. 모든 쿼리를 즐겨찾기에 저장할 수 있습니다. 즐겨찾기에 쿼리를 저장, 하 여 쿼리가 자동으로 실행 될 때마다를 **사냥** 페이지에 액세스 합니다. 고유한 hunting 쿼리 또는 복제 만들고 기존 hunting 쿼리 템플릿을 사용자 지정할 수 있습니다. 
 
2. 클릭 **쿼리 실행** 사냥 페이지를 떠나지 않고 쿼리를 실행 하려면 쿼리 세부 정보 페이지를 찾거나에서.  테이블 내에서 일치 항목 수가 표시 됩니다. Hunting 쿼리와 일치 하는 해당 항목의 목록을 검토 합니다. 일치 하는 연관 된 kill 체인의 단계를 확인 합니다.

3. 쿼리 세부 정보 창에서 기본 쿼리는 빠른 검토를 수행 하거나 클릭 **쿼리 결과 보려면** 를 Log Analytics에서 쿼리를 엽니다. 아래쪽에서 쿼리에 대 한 일치 항목을 검토 합니다.

4.  선택한 행에서 클릭 **추가 책갈피** 좀 더 조사할 필요가-행을 추가 하려면 이렇게 의심 스 럽 지 모든 항목입니다. 

5. 그런 다음 주로 다시 이동 **사냥** 페이지를 클릭 합니다 **책갈피** 모든 의심 스러운 활동 탭입니다. 

6. 책갈피를 선택 하 고 클릭 **조사** 조사 환경이 열려고 합니다. 책갈피를 필터링 할 수 있습니다. 예를 들어, 캠페인을 조사 하려는 경우 수 캠페인에 대 한 태그를 만들고 캠페인에 따라 모든 책갈피를 필터링 합니다.

1. Hunting 쿼리 제공 가능한 공격에 대 한 중요 정보를 검색 한 후에 규칙 쿼리를 기반으로 하며 이러한 가치를 경고로 보안 인시던트 응답자 프로그램에 사용자 지정 검색을 만들 수 있습니다.

 

## <a name="query-language"></a>쿼리 언어 

Azure Sentinel에서 찾거나 Azure Log Analytics 쿼리 언어를 기반으로 합니다. 쿼리 언어 및 지원 되는 연산자에 대 한 자세한 내용은 참조 하세요. [쿼리 언어 참조](https://docs.loganalytics.io/docs/Language-Reference/)합니다.

## <a name="public-hunting-query-github-repository"></a>공용 hunting 쿼리 GitHub 리포지토리

체크 아웃 합니다 [사냥 쿼리 리포지토리](https://github.com/Azure/Orion)합니다. 참여 하 고 고객이 공유 하는 예제 쿼리를 사용 합니다.

 

## <a name="sample-query"></a>샘플 쿼리

일반적인 쿼리는 테이블 이름 뒤에 일련의 구분 하 여 연산자를 사용 하 여 시작 \|합니다.

시작 테이블을 사용 하 여 위의 예에서 SecurityEvent 이름을 지정 하 고 필요에 따라 파이프 된 요소를 추가 합니다.

1. 지난 7 일 레코드만 검토 하는 시간 필터를 정의 합니다.

2. 이벤트 ID 4688 표시 하도록 쿼리에 필터를 추가 합니다.

3. Cscript.exe의 인스턴스만 포함할 명령줄에서 쿼리에 필터를 추가 합니다.

4. 1000으로 결과 제한 및 클릭 탐색에 관심이 열만 프로젝트 **쿼리 실행**합니다.
5. 녹색 삼각형을 클릭 하 고 쿼리를 실행 합니다. 쿼리를 테스트 하 고 실행 하 여 비정상적인 동작을 검색할 수 있습니다.

## <a name="useful-operators"></a>유용한 연산자

많은 사용 가능한 연산자에 강력한 쿼리 언어 이며, 몇 가지 유용한 연산자는 다음과 같습니다.

**여기서** -조건자를 만족 하는 행의 하위 집합 테이블을 필터링 합니다.

**요약** -입력된 테이블의 콘텐츠를 집계 하는 테이블을 생성 합니다.

**조인** -각 테이블에서 지정 된 열의 값을 비교 하 여 새 테이블을 형성 하 여 두 테이블의 행을 병합 합니다.

**개수** -입력된 레코드 집합의 레코드 수를 반환 합니다.

**상위** -첫 번째 N 반환 레코드를 지정 된 열을 기준으로 정렬 합니다.

**제한** -지정 된 행 수를 반환 합니다.

**프로젝트** -열을 포함, 이름 바꾸기 또는 삭제를 선택 하 고 새 계산된 열을 삽입 합니다.

**확장** -계산된 열을 만들고 결과 집합에 추가 합니다.

**makeset** -그룹의 동적 (JSON) 배열을 Expr 사용 하는 고유 값의 집합을 반환

**찾을** -테이블의 여러 조건자와 일치 하는 행을 찾습니다.

## <a name="save-a-query"></a>쿼리 저장

만들거나 수 있습니다 또는 쿼리를 수정 하 고 고유한 쿼리로 저장 동일한 테 넌 트에 사용자와 공유 합니다.

   ![쿼리 저장](./media/tutorial-hunting/save-query.png)

새 hunting 쿼리를 만듭니다.

1. 클릭 **새 쿼리** 선택한 **저장**합니다.
2. 모든 빈 필드에 입력 하 고 선택 **저장할**합니다.

   ![새 쿼리](./media/tutorial-hunting/new-query.png)

복제 하 고 기존 hunting 쿼리를 수정 합니다.

1. 수정 하려는 테이블에서 찾거나 쿼리를 선택 합니다.
2. 쿼리를 수정 하 고 선택 원하는 줄에서 줄임표 (...)를 선택 **복제 쿼리**합니다.

   ![쿼리 복제](./media/tutorial-hunting/clone-query.png)
 

3. 선택한 쿼리를 수정 **만들기**합니다.

   ![사용자 지정 쿼리](./media/tutorial-hunting/custom-query.png)

## <a name="next-steps"></a>다음 단계
이 문서에서는 Azure Sentinel hunting 조사를 실행 하는 방법을 알아보았습니다. Azure Sentinel에 대한 자세한 내용은 다음 문서를 참조하세요.


- [Notebook을 사용 하 여 자동화 된 hunting 캠페인 실행](notebooks.md)
- [책갈피를 사용 하 여 사냥 하는 동안 흥미로운 정보를 저장 합니다.](bookmarks.md)

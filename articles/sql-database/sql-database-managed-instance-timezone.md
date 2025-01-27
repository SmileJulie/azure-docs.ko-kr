---
title: Azure SQL Database Managed Instance 시간대 | Microsoft Docs "
description: Azure SQL Database Managed Instance의 표준 시간대 세부 사항에 알아봅니다.
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
manager: craigg
ms.date: 07/05/2019
ms.openlocfilehash: 05ec49c98c5bcfe40346550f5570c03a8fb3f881
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657985"
---
# <a name="time-zones-in-azure-sql-database-managed-instance"></a>Azure SQL Database Managed Instance의 표준 시간대

Utc (협정 세계시)는 클라우드 솔루션의 데이터 계층에 대 한 권장 되는 표준 시간대입니다. 또한 azure SQL Database Managed Instance 표준 시간대의 날짜 및 시간 값을 저장 하 고 특정 표준 시간대의 암시적 컨텍스트를 사용 하 여 날짜 및 시간 함수를 호출 하는 기존 응용 프로그램의 요구에 맞게 선택할을 제공 합니다.

T-SQL 함수 [getdate ()](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql) 하거나 CLR 코드 수준 인스턴스에서 설정 하는 표준 시간대를 관찰 합니다. 또한 SQL Server 에이전트 작업 인스턴스의 표준 시간대에 따라 일정을 따릅니다.

  >[!NOTE]
  > 관리 되는 인스턴스는 표준 시간대 설정을 지 원하는 Azure SQL Database의 전용 배포 옵션입니다. 다른 배포 옵션에는 항상 UTC 따라야합니다.
사용 하 여 [AT TIME ZONE](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql) 아닌 UTC 표준 시간대의 날짜 및 시간 정보를 해석 하는 경우 단일 및 풀링된 SQL 데이터베이스에 있습니다.

## <a name="supported-time-zones"></a>지원 되는 표준 시간대

지원 되는 표준 시간대의 집합을 관리 되는 인스턴스의 기본 운영 체제에서 상속 됩니다. 새 표준 시간대 정의 가져오고 기존 변경 내용을 반영을 정기적으로 업데이트 됩니다.

[일광 절약 시간/표준 시간대 변경 정책](https://aka.ms/time) 2010 앞에서 기록 정확도 보장 합니다.

지원 되는 표준 시간대 이름의 목록을 통해 노출 되는 [sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) 시스템 뷰.

## <a name="set-a-time-zone"></a>표준 시간대 설정

관리 되는 인스턴스의 표준 시간대만 인스턴스를 만드는 동안 설정할 수 있습니다. 기본 표준 시간대는 UTC입니다.

  >[!NOTE]
  > 기존 관리 되는 인스턴스의 표준 시간대를 변경할 수 없습니다.

### <a name="set-the-time-zone-through-the-azure-portal"></a>Azure portal 통해 표준 시간대 설정

새 인스턴스에 대 한 매개 변수를 입력 하면 지원 되는 표준 시간대의 목록에서 표준 시간대를 선택 합니다.
  
![인스턴스를 만드는 동안 표준 시간대 설정](media/sql-database-managed-instance-timezone/01-setting_timezone-during-instance-creation.png)

### <a name="azure-resource-manager-template"></a>Azure Resource Manager 템플릿

된 timezoneId 속성을 지정 하 [Resource Manager 템플릿을](https://aka.ms/sql-mi-create-arm-posh) 인스턴스를 만드는 동안 표준 시간대를 설정 합니다.

```json
"properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen5",
                "collation": "Serbian_Cyrillic_100_CS_AS",
                "timezoneId": "Central European Standard Time"
            },

```

된 timezoneId 속성에 대해 지원 되는 값의 목록은이 문서의 끝입니다.

지정 하지 않으면 표준 시간대는 UTC로 설정 됩니다.

## <a name="check-the-time-zone-of-an-instance"></a>인스턴스의 표준 시간대를 확인 합니다.

합니다 [CURRENT_TIMEZONE](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql) 함수 인스턴스의 표준 시간대의 표시 이름을 반환 합니다.

## <a name="cross-feature-considerations"></a>기능 간 고려 사항

### <a name="restore-and-import"></a>복원 및 가져오기

백업 파일을 복원할 수도 있고 다른 표준 시간대 설정을 사용 하 여 관리 되는 인스턴스를 인스턴스 또는 서버에서 데이터를 가져올 수 있습니다. 주의 해 서 사용 해야 합니다. 다른 표준 시간대 설정 사용 하 여 두 SQL Server 인스턴스 간에 데이터를 전송 하는 경우와 마찬가지로 응용 프로그램 동작 및 쿼리 및 보고서의 결과 분석 합니다.

### <a name="point-in-time-restore"></a>지정 시간 복원

<del>지정 시간 복원을 수행할 때에 복원 시간을 UTC 시간으로 해석 됩니다. 이 설정은 일광 절약 시간 및 해당 잠재적인 변경으로 인해 모호성을 방지할 수 있습니다.<del>

 >[!WARNING]
  > 현재 동작에 따라 위의 문을 아니며 시간으로 복원 하는 자동 데이터베이스 백업을에서 원본 관리 되는 인스턴스의 표준 시간대에 따라 해석 됩니다. 지점 지정 된 시간 UTC 시간으로 해석 하도록이 동작을 수정 노력 합니다. 참조 [알려진 문제](sql-database-managed-instance-timezone.md#known-issues) 대 한 자세한 내용은 합니다.

### <a name="auto-failover-groups"></a>자동 장애 조치 그룹

장애 조치 그룹에서 기본 및 보조 인스턴스에서 동일한 표준 시간대를 사용 하 여 적용 되지 않습니다 하지만 강력한 좋습니다.

  >[!WARNING]
  > 장애 조치 그룹에서 기본 및 보조 인스턴스에 대 한 동일한 표준 시간대를 사용 하는 것이 좋습니다. 일부 드문 시나리오 인해 적용 되지 않으며 기본 및 보조 인스턴스에서 동일한 표준 시간대를 유지 합니다. 수동 또는 자동 장애 조치의 경우 보조 인스턴스를 유지 하는 원본 표준 시간대를 이해 하는 것이 반드시 합니다.

## <a name="limitations"></a>제한 사항

- 기존 관리 되는 인스턴스의 표준 시간대를 변경할 수 없습니다.
- SQL Server 에이전트 작업에서 시작 하는 외부 프로세스 인스턴스의 표준 시간대를 관찰 하지 않습니다.

## <a name="known-issues"></a>알려진 문제

지정 시간 복원 (PITR) 작업이 수행 되 면를 복원 하는 시간 PITR에 대 한 포털 페이지 시간이 UTC로 해석 되는 제안 하는 경우에 관리 되는 인스턴스에 자동 데이터베이스 백업을 사용을 설정 하는 표준 시간대에 따라 해석 됩니다.

예제:

예를 들어 여기서 자동 백업을에서 해당 인스턴스 (UTC-5) 동부 표준시 표준 시간대 설정을 있습니다.
지정 시간 복원에 대 한 포털 페이지를 복원 하려면 선택 하는 시간을 UTC 시간을 제안 합니다.

![포털을 사용 하는 현지 시간을 사용 하 여 PITR](media/sql-database-managed-instance-timezone/02-pitr-with-nonutc-timezone.png)

그러나를 복원 하는 시간은 실제로 동부 표준시로 해석 됩니다 하 고이 특정 예제의 데이터베이스 9 시 동부 표준시, 및 UTC 시간이 아닌 상태로 복원 됩니다.

UTC 시간에 특정 시점으로 특정 시점 복원을 수행 하려는 경우 먼저 원본 인스턴스의 표준 시간대의 해당 시간을 계산 하 고 포털 또는 PowerShell/CLI 스크립트에서 해당 시간을 사용 합니다.

## <a name="list-of-supported-time-zones"></a>지원 되는 표준 시간대의 목록

| **표준 시간대 ID** | **표준 시간대 표시 이름** |
| --- | --- |
| 날짜 변경 선 표준시 | (UTC-12:00) 날짜변경선 서쪽 |
| UTC-11 | (UTC-11:00) 협정세계시-11 |
| 알류샨 표준시 | (UTC-10:00) 알류샨 열도 |
| 하와이 표준시 | (UTC-10:00) 하와이 |
| 마키저스 표준시 | (UTC-09:30) 마르케사스 제도 |
| 알래스카 표준시 | (UTC-09:00) 알래스카 |
| UTC-09 | (UTC-09:00) 협정세계시-09 |
| 태평양 표준시 (멕시코) | (UTC-08:00) 바하칼리포르니아 |
| UTC-08 | (UTC-08:00) 협정세계시-08 |
| 태평양 표준시 | (UTC-08:00) 태평양 표준시(미국과 캐나다) |
| 미국 산지 표준시 | (UTC-07:00) 애리조나 |
| 산지 표준시 (멕시코) | (UTC-07:00) 치와와, 라파스, 마사틀란 |
| 산지 표준시 | (UTC-07:00) 산지 표준시(미국과 캐나다) |
| 중앙 아메리카 표준시 | (UTC-06:00) 중앙 아메리카 |
| 중부 표준시 | (UTC-06:00) 중부 표준시(미국과 캐나다) |
| 이스터 섬 표준시 | (UTC-06:00) 이스터 섬 |
| 중부 표준시 (멕시코) | (UTC-06:00) 과달라하라, 멕시코시티, 몬테레이 |
| 캐나다 중부 표준시 | (UTC-06:00) 서스캐처원 |
| SA 태평양 표준시 | (UTC-05:00) 보고타, 리마, 키토, 리오 브랑코 |
| 동부 표준시 (멕시코) | (UTC-05:00) 체투말 |
| 동부 표준시 | (UTC-05:00) 동부 표준시(미국과 캐나다) |
| 아이티 표준시 | (UTC-05:00) 아이티 |
| 쿠바 표준시 | (UTC-05:00) 하바나 |
| 미국 동부 표준시 | (UTC-05:00) 인디애나(동부) |
| 터크스 케이커스 표준시 | (UTC-05:00) 터크스 케이커스 |
| 파라과이 표준시 | (UTC-04:00) 아순시온 |
| 대서양 표준시 | (UTC-04:00) 대서양 표준시(캐나다) |
| 베네수엘라 표준시 | (UTC-04:00) 카라카스 |
| 브라질 중부 표준시 | (UTC-04:00) 쿠이아바 |
| SA 서 부 표준시 | (UTC-04:00) 조지타운, 라파스, 마나우스, 산후안 |
| 태평양 SA 표준시 | (UTC-04:00) 산티아고 |
| 뉴펀들랜드 표준시 | (UTC-03:30) 뉴펀들랜드 |
| 토칸칭스 표준시 | (UTC-03:00) 아라구아이나 |
| 5\. 남아메리카 표준시 | (UTC-03:00) 브라질리아 |
| SA 동부 표준시 | (UTC-03:00) 카옌, 포르탈레자 |
| 아르헨티나 표준 시간 | (UTC-03:00) 부에노스아이레스 |
| 그린란드 표준시 | (UTC-03:00) 그린란드 |
| 몬테비디오 표준시 | (UTC-03:00) 몬테비데오 |
| Magallanes 표준시 | (UTC-03:00) 푼 타 아레나스 |
| 생피에르 표준시 | (UTC-03:00) 생피에르앤드미클롱 |
| 바이아 표준시 | (UTC-03:00) 살바도르 |
| UTC-02 | (UTC-02:00) 협정세계시-02 |
| 대서양 표준시 | (UTC-02:00) 중부-대서양 - 이전 |
| 아조레스 표준시 | (UTC-01:00) 아조레스 |
| 카보베르데 표준시 | (UTC-01:00) 카보베르데 제도 |
| UTC | (UTC) 협정세계시 |
| GMT 표준시 | (UTC+00:00) 더블린, 에딘버러, 리스본, 런던 |
| 그리니치 표준시 | (UTC+00:00) 몬로비아, 레이캬비크 |
| W. 유럽 표준시 | (UTC+01:00) 암스테르담, 베를린, 베른, 로마, 스톡홀름, 빈 |
| 중앙 유럽 표준시 | (UTC+01:00) 베오그라드, 브라티슬라바, 부다페스트, 류블랴나, 프라하 |
| 로망스 표준시 | (UTC+01:00) 브뤼셀, 코펜하겐, 마드리드, 파리 |
| 모로코 표준 시간 | (UTC + 01시) 카사블랑카 |
| 상파울루 관련 저서인 표준시 | (UTC + 01시) 상파울루 보면 |
| 중앙 유럽 표준시 | (UTC+01:00) 사라예보, 스코페, 바르샤바, 자그레브 |
| W. 중앙 아프리카 표준시 | (UTC+01:00) 서중앙 아프리카 |
| 요르단 표준시 | (UTC+02:00) 암만 |
| GTB 표준시 | (UTC+02:00) 아테네, 부쿠레슈티 |
| 중동 표준시 | (UTC+02:00) 베이루트 |
| 이집트 표준시 | (UTC+02:00) 카이로 |
| 5\. 유럽 표준시 | (UTC+02:00) 키시나우 |
| 시리아 표준시 | (UTC+02:00) 다마스쿠스 |
| 팔레스타인 표준시 | (UTC+02:00) 가자, 헤브론 |
| 남아프리카 표준시 | (UTC+02:00) 하라레, 프리토리아 |
| FLE 표준시 | (UTC+02:00) 헬싱키, 키예프, 리가, 소피아, 탈린, 빌뉴스 |
| 이스라엘 표준시 | (UTC+02:00) 예루살렘 |
| 칼리닌그라드 표준시 | (UTC+02:00) 칼리닌그라드 |
| 수단 표준시 | (UTC + 02시) 하르툼 |
| 리비아 표준시 | (UTC+02:00) 트리폴리 |
| 나미비아 표준시 | (UTC + 02시) 빈트후크 |
| 아랍 표준시 | (UTC+03:00) 바그다드 |
| 터키 표준시 | (UTC + 03시) 이스탄불 |
| 아랍 표준시 | (UTC+03:00) 쿠웨이트, 리야드 |
| 벨라루스 표준시 | (UTC+03:00) 민스크 |
| 러시아 표준시 | (UTC + 03시) 모스크바, 상트페테르부르크 |
| 5\. 아프리카 표준시 | (UTC+03:00) 나이로비 |
| 이란 표준시 | (UTC+03:30) 테헤란 |
| 아랍 표준시 | (UTC+04:00) 아부다비, 무스카트 |
| 아스트라한 표준시 | (UTC+04:00) 아스트라칸, 울랴노프스크 |
| 아제르바이잔 표준시 | (UTC+04:00) 바쿠 |
| 러시아 시간대 3 | (UTC+04:00) 이제프스크, 사마라 |
| 모리셔스 표준 시간 | (UTC+04:00) 포트루이스 |
| 사라토프 표준시 | (UTC + 04시) 사라토프 |
| 조지아 표준시 | (UTC+04:00) 트빌리시 |
| 볼고그라드 표준시 | (UTC + 04시) 볼고그라드 |
| 코코서스 표준시 | (UTC+04:00) 예레반 |
| 아프가니스탄 표준시 | (UTC+04:30) 카불 |
| 서 아시아 표준시 | (UTC+05:00) 아슈하바트, 타슈켄트 |
| 예카테린부르크 표준시 | (UTC+05:00) 예카테린부르크 |
| 파키스탄 표준 시간 | (UTC+05:00) 이슬라마바드, 카라치 |
| 인도 표준시 | (UTC+05:30) 첸나이, 콜카타, 뭄바이, 뉴델리 |
| 스리랑카 표준시 | (UTC+05:30) 스리자야와르데네푸라 |
| 네팔 표준시 | (UTC+05:45) 카트만두 |
| 중앙 아시아 표준시 | (UTC+06:00) 아스타나 |
| 방글라데시 표준시 | (UTC+06:00) 다카 |
| 옴스크 표준시 | (UTC + 06시) 옴스크 |
| 미얀마 표준시 | (UTC+06:30) 양곤(랑군) |
| SE 아시아 표준시 | (UTC+07:00) 방콕, 하노이, 자카르타 |
| 알타이 표준시 | (UTC+07:00) 바르나울, 고르노알타이스크 |
| W. 몽골 표준시 | (UTC+07:00) 호브드 |
| 북아시아 표준시 | (UTC+07:00) 크라스노야르스크 |
| N. 중앙 아시아 표준시 | (UTC + 07시) 노보시비르스크 |
| 톰스크 표준시 | (UTC+07:00) 톰스크 |
| 중국 표준시 | (UTC+08:00) 베이징, 충칭, 홍콩, 우루무치 |
| 북아시아 동부 표준시 | (UTC+08:00) 이르쿠츠크 |
| 싱가포르 표준시 | (UTC+08:00) 쿠알라룸푸르, 싱가포르 |
| W. 오스트레일리아 표준시 | (UTC+08:00) 퍼스 |
| 타이베이 표준시 | (UTC+08:00) 타이베이 |
| 울란바토르 표준시 | (UTC+08:00) 울란바토르 |
| 오스트레일리아 중부 표준시 | (UTC+08:45) 유클라 |
| 트 란 스 바이칼 표준시 | (UTC+09:00) 치타 |
| 도쿄 표준시 | (UTC+09:00) 오사카, 삿포로, 도쿄 |
| 북한 표준시 | (UTC + 09시) 평양 |
| 대한민국 표준시 | (UTC+09:00) 서울 |
| 야쿠츠크 표준시 | (UTC+09:00) 야쿠츠크 |
| 중부 오스트레일리아 표준시 | (UTC+09:30) 애들레이드 |
| 오스트레일리아 중부 표준시 | (UTC+09:30) 다윈 |
| 5\. 오스트레일리아 표준시 | (UTC+10:00) 브리즈번 |
| 오스트레일리아 동부 표준시 | (UTC+10:00) 캔버라, 멜버른, 시드니 |
| 서 태평양 표준시 | (UTC+10:00) 괌, 포트모르즈비 |
| 태즈메이니아 표준시 | (UTC+10:00) 호바트 |
| 블라디보스토크 표준시 | (UTC+10:00) 블라디보스토크 |
| 로드하우 표준시 | (UTC+10:30) 로드하우 섬 |
| 부건빌 표준시 | (UTC+11:00) 부건빌 섬 |
| 러시아 시간대 10 | (UTC+11:00) 초쿠르다흐 |
| 마가단 표준시 | (UTC+11:00) 마가단 |
| 노퍽 표준시 | (UTC+11:00) 노퍽 섬 |
| 사할린 표준시 | (UTC+11:00) 사할린 |
| 중앙 태평양 표준시 | (UTC+11:00) 솔로몬 제도, 뉴칼레도니아 |
| 러시아 시간대 11 | (UTC+12:00) 아나디리, 페트로파블로프스크-캄차스키 |
| 뉴질랜드 표준시 | (UTC+12:00) 오클랜드, 웰링턴 |
| UTC + 12 | (UTC+12:00) 협정세계시+12 |
| 피지 표준시 | (UTC+12:00) 피지 |
| 캄차카 반도 표준시 | (UTC+12:00) 페트로파블로프스크-캄차스키 - 이전 |
| 채텀 섬 표준시 | (UTC+12:45) 체텀 제도 |
| UTC + 13 | (UTC + 13시) 협정 세계시 + 13 |
| 통가 표준시 | (UTC+13:00) 누쿠알로파 |
| 사모아 표준시 | (UTC+13:00) 사모아 |
| 라인 제도 표준시 | (UTC+14:00) 키리티마티 섬 |

## <a name="see-also"></a>참고자료 

- [CURRENT_TIMEZONE (Transact SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql)
- [AT TIME ZONE (TRANSACT-SQL)](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)
- [sys.time_zone_info (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql)

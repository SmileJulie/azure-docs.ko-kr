---
title: Azure CDN(Content Delivery Network) 제품 기능 비교 | Microsoft Docs
description: 각 Azure CDN(Content Delivery Network) 제품이 지원하는 기능에 대해 알아봅니다.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: mdgattuso
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 06/05/2019
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: e34fbe200420fef1653e21bb44ac2f7e0de90fb7
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593865"
---
# <a name="compare-azure-cdn-product-features"></a>Azure CDN 제품 기능 비교

Azure CDN(Content Delivery Network)에는 다음과 같은 네 가지 제품이 포함됩니다. **Microsoft의 Azure CDN 표준**, **Akamai의 Azure CDN 표준**, **Verizon의 Azure CDN 표준** 및 **Verizon의 Azure CDN 프리미엄**. **Verizon의 Azure CDN Standard** 프로필을 **Verizon의 Azure CDN Premium**으로 마이그레이션하는 방법은 [Azure CDN 프로필을 Standard Verizon에서 Premium Verizon으로 마이그레이션](cdn-migrate.md)을 참조하세요.

다음 표는 각 제품에 사용할 수 있는 기능을 비교합니다.

| **성능 기능 및 최적화** | **표준 Microsoft** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** |
| --- | --- | --- | --- | --- |
| [동적 사이트 가속](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration)  | [Azure Front Door Service](https://docs.microsoft.com/azure/frontdoor/front-door-overview)를 통해 제공됨 | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[동적 사이트 가속 - 적응 이미지 압축](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-azure-cdn-from-akamai-only)  |  | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[동적 사이트 가속 - 개체 프리페치](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-azure-cdn-from-akamai-only)  |  | **&#x2713;**  |  |  |
| [일반 웹 배달 최적화](https://docs.microsoft.com/azure/cdn/cdn-optimization-overview#general-web-delivery)  | **&#x2713;** | **&#x2713;** , 평균 파일 크기가 10MB보다 작으면 이 최적화 유형을 선택합니다.  | **&#x2713;** |  **&#x2713;** |
| [비디오 스트리밍 최적화](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization)  | 일반 웹 배달을 통해 | **&#x2713;**  | 일반 웹 배달을 통해 |  일반 웹 배달을 통해 |
| [큰 파일 최적화](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization)  | 일반 웹 배달을 통해 | **&#x2713;** , 평균 파일 크기가 10MB보다 크면 이 최적화 유형을 선택합니다.   | 일반 웹 배달을 통해 |  일반 웹 배달을 통해 |
| 최적화 유형 변경 | |**&#x2713;** | | |
| 원본 포트 |모든 TCP 포트 |[허용된 원본 포트](https://docs.microsoft.com/previous-versions/azure/mt757337(v%3Dazure.100)#allowed-origin-ports) |모든 TCP 포트 |모든 TCP 포트 |
| [GSLB(전역 서버 부하 분산)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [빠른 삭제](cdn-purge-endpoint.md)  | **&#x2713;** |**&#x2713;** , 모두 제거 및 와일드카드 제거는 현재 Akamai의 Azure CDN에서 지원되지 않습니다. |**&#x2713;** |**&#x2713;** |
| [자산 미리 로드](cdn-preload-endpoint.md)  |  | |**&#x2713;** |**&#x2713;** |
| 캐시/헤더 설정([캐싱 규칙](cdn-caching-rules.md)사용)  |  |**&#x2713;** |**&#x2713;** | |
| 사용자 지정이 가능한 규칙 기반의 콘텐츠 배달 엔진([규칙 엔진](cdn-rules-engine.md) 사용)  |  | | |**&#x2713;** |
| 캐시/헤더 설정( [규칙 엔진](cdn-rules-engine.md)사용)  |  | | |**&#x2713;** |
| URL 리디렉션/다시 쓰기( [규칙 엔진](cdn-rules-engine.md)사용)  |  | | |**&#x2713;** |
| 모바일 디바이스 규칙( [규칙 엔진](cdn-rules-engine.md)사용)  |  | | |**&#x2713;** |
| [쿼리 문자열 캐싱](cdn-query-string.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| IPv4/IPv6 이중 스택 | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTP/2 지원](cdn-http2.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **보안** | **표준 Microsoft** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| CDN 엔드포인트에 HTTPS 지원 | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [사용자 지정 도메인 HTTPS](cdn-custom-ssl.md)  | **&#x2713;** | **&#x2713;** 활성화하려면 직접 CNAME이 필요함 |**&#x2713;** |**&#x2713;** |
| [사용자 지정 도메인 이름 지원](cdn-map-content-to-custom-domain.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [지역 필터링](cdn-restrict-access-by-country.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [토큰 인증](cdn-token-auth.md)  |  |  |  |**&#x2713;**| 
| [DDOS 보호](https://www.us-cert.gov/ncas/tips/ST04-015)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [사용자 고유의 인증서 가져오기](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates) |**&#x2713;** |  | **&#x2713;** | **&#x2713;** |
||||
| **분석 및 보고** | **표준 Microsoft** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| [Azure 진단 로그](cdn-azure-diagnostic-logs.md)  | **&#x2713;** | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Verizon의 핵심 보고서](cdn-analyze-usage-patterns.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Verizon의 사용자 지정 보고서](cdn-verizon-custom-reports.md)  |  | |**&#x2713;** |**&#x2713;** |
| [고급 HTTP 보고서](cdn-advanced-http-reports.md)  |  | | |**&#x2713;** |
| [실시간 통계](cdn-real-time-stats.md)  |  | | |**&#x2713;** |
| [에지 노드 성능](cdn-edge-performance.md)  |  | | |**&#x2713;** |
| [실시간 경고](cdn-real-time-alerts.md)  |  | | |**&#x2713;** |
||||
| **사용 편의성** | **표준 Microsoft** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| [Storage](cdn-create-a-storage-account-with-cdn.md), [Web Apps](cdn-add-to-web-app.md) 및 [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) 등의 Azure 서비스와 손쉬운 통합  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [REST API](/rest/api/cdn/), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) 또는 [PowerShell](cdn-manage-powershell.md)을 통한 관리  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [압축 MIME 형식](https://docs.microsoft.com/azure/cdn/cdn-improve-performance)  |기본값만 |구성 가능 여부 |구성 가능 여부  |구성 가능 여부  |
| 압축 인코딩  |gzip, brotli |gzip |gzip, deflate, bzip2, brotili  |gzip, deflate, bzip2, brotili  |







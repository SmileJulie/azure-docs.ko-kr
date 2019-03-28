---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/18/2019
ms.author: alkohli
ms.openlocfilehash: 05673885336dbfb9a70843bd0ccc4e700091ad7e
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58405628"
---
|     오류 코드     |      오류 설명     |
|--------------------|--------------------------|
|    100             | 컨테이너 또는 공유 이름은 3자~63자여야 합니다.|
|    101             | 컨테이너 또는 공유 이름은 문자, 숫자 또는 하이픈만으로 구성되어야 합니다.|
|    102             | 컨테이너 또는 공유 이름은 문자, 숫자 또는 하이픈만으로 구성되어야 합니다.|
|    103             | Blob 또는 파일 이름에 지원되지 않는 제어 문자가 포함되어 있습니다.|
|    104             | Blob 또는 파일 이름에 잘못된 문자가 포함되어 있습니다.|
|    105             | Blob 또는 파일 이름에 세그먼트가 너무 많습니다. (각 세그먼트는 슬래시-/로 구분됩니다).|
|    106             | Blob 또는 파일 이름이 너무 깁니다.|
|    107             | Blob 또는 파일의 세그먼트 중 하나가 너무 깁니다. |
|    108             | 파일 크기가 업로드할 수 있는 최대 파일 크기를 초과합니다.    |
|    109             | Blob 또는 파일이 잘못 정렬되었습니다.  |
|    110             | 유니코드로 인코딩된 파일 이름 또는 Blob이 유효하지 않습니다.|
|    111             | 파일 또는 Blob의 이름 또는 접두사가 예약 된 이름이어서 지원되지 않습니다(예: COM1).|
|    2000            | Etag 불일치가 클라우드의 블록 Blob과 디바이스의 블록 Blob 사이에 충돌이 있음을 나타냅니다. 이 충돌을 해결하려면 해당 파일 중 클라우드의 버전이나 디바이스의 버전 중 하나를 삭제하십시오.    |
|    2001            | 파일을 업로드한 후 파일을 처리하는 동안 예기치 않은 문제가 발생했습니다.    이 오류가 표시되고 24시간 넘게 지속되면 고객 지원팀에 문의하십시오. |
|    2002            | 파일이 다른 프로세스에 이미 열려 있기 때문에 핸들이 닫힐 때까지 업로드할 수 없습니다.|
|    2003            | 업로드할 파일을 열 수 없습니다. 이 오류가 표시되면 Microsoft 지원에 문의하세요.|
|    2004            | 데이터를 업로드할 컨테이너에 연결할 수 없습니다.|
|    2005            | 계정 권한이 잘못되었거나 만료되어 컨테이너에 연결할 수 없습니다. 액세스 권한을 확인하십시오.|
|    2006            | 계정 또는 공유가 비활성화되어 있어 계정에 데이터를 업로드할 수 없습니다.|
|    2007            | 계정 권한이 잘못되었거나 만료되어 컨테이너에 연결할 수 없습니다. 액세스 권한을 확인하십시오.|
|    2008            | 컨테이너가 가득 차서 새 데이터를 추가할 수 없습니다. 유형에 따라 지원되는 컨테이너 크기는 Azure 사양을 확인하십시오. 예를 들어, Azure File에서 지원되는 최대 파일 크기는 5TB입니다.|
|    2009            | 공유와 연결된 컨테이너가 존재하지 않아 데이터를 업로드할 수 없습니다.|    
|    2997            | 예기치 않은 오류가 발생했습니다. 자체적으로 해결되는 일시적인 오류입니다.|
|    2998            | 예기치 않은 오류가 발생했습니다. 오류가 자체적으로 해결될 수 있지만 24시간 넘게 지속되는 경우에는 Microsoft 지원에 문의하세요.|
|    16000           | 이 파일을 가져올 수 없습니다.|
|    16001           | 이 파일이 로컬 시스템에 이미 있기 때문에 가져올 수 없습니다.|
|    16002           |이 파일이 완전히 업로드되지 않았기 때문에 새로 고칠 수 없습니다.|

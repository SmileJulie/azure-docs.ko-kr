---
title: 포함 파일
description: 포함 파일
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 03/20/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: da63a5418ab94623f6ce3c9f35a085dd8b198d1a
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67182761"
---
이 자습서를 완료하려면 다음 요구 사항을 충족해야 합니다.

**Azure CLI**: 로컬 컴퓨터에 Azure CLI 버전 2.0.29 이상이 설치되어 있어야 합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드가 필요한 경우 [Azure CLI 설치][azure-cli-install]를 참조하세요.

**Docker**: 이 자습서에서는 컨테이너, 컨테이너 이미지 및 기본 `docker` 명령과 같은 핵심 Docker 개념에 대한 기본적인 지식이 있다고 가정합니다. Docker 및 컨테이너에 대한 기본 사항은 [Docker 개요][docker-get-started]를 참조하세요.

**Docker 엔진**: 이 자습서를 완료하려면 로컬에 Docker 엔진을 설치해야 합니다. Docker는 [macOS][docker-mac], [Windows][docker-windows] 및 [Linux][docker-linux]에 Docker 환경을 구성하는 패키지를 제공합니다.

> [!IMPORTANT]
> Azure Cloud Shell에는 Docker 디먼이 포함되어 있지 않으므로 이 자습서를 완료하려면 *반드시* *로컬 컴퓨터*에 Azure CLI와 Docker 엔진을 모두 설치해야 합니다. 이 자습서에는 Azure Cloud Shell을 사용할 수 없습니다.

<!-- LINKS - External -->
[docker-get-started]: https://docs.docker.com/engine/docker-overview/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli

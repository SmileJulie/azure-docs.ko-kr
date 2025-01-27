---
title: Azure Storage를 사용하여 클라우드에 이미지 데이터 업로드 | Microsoft Docs
description: 웹앱에서 Azure Blob Storage를 사용하여 앱 데이터를 저장합니다.
services: storage
author: normesta
ms.service: storage
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: normesta
ms.reviewer: seguler
ms.custom: mvc
ms.openlocfilehash: 612c0b48faac365623fe36d6d2435c1c79566d9b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071318"
---
# <a name="tutorial-upload-image-data-in-the-cloud-with-azure-storage"></a>자습서: Azure Storage를 사용하여 클라우드에 이미지 데이터 업로드

이 자습서는 시리즈의 1부입니다. 이 자습서에서는 Azure Storage 클라이언트 라이브러리를 사용하여 이미지를 스토리지 계정에 업로드하는 웹앱을 배포하는 방법에 대해 알아봅니다. 완료되면 Azure Storage에서 이미지를 저장 및 표시하는 웹앱이 갖추어집니다.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)
![.NET 이미지 크기 조정 앱](media/storage-upload-process-images/figure2.png)

# <a name="nodejs-v2-sdktabnodejs"></a>[Node.js V2 SDK](#tab/nodejs)
![Node.js V2 이미지 크기 조정 앱](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)
![Node.js V10 이미지 크기 조정 앱](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

시리즈 1부에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 저장소 계정 만들기
> * 컨테이너 만들기 및 사용 권한 설정
> * 액세스 키 가져오기
> * Azure에 웹앱 배포
> * 앱 설정 구성
> * 웹앱과 상호 작용

## <a name="prerequisites"></a>필수 조건

이 자습서를 완료하려면 Azure 구독이 필요합니다. 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)을 만듭니다.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI를 로컬로 설치하여 사용하려면 이 자습서에서 Azure CLI 버전 2.0.4 이상을 실행해야 합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드가 필요한 경우, [Azure CLI 설치]( /cli/azure/install-azure-cli)를 참조하세요. 

## <a name="create-a-resource-group"></a>리소스 그룹 만들기 

[az group create](/cli/azure/group) 명령을 사용하여 리소스 그룹을 만듭니다. Azure 리소스 그룹은 Azure 리소스가 배포 및 관리되는 논리적 컨테이너입니다.  

다음 예제에서는 `myResourceGroup`이라는 리소스 그룹을 만듭니다.

```azurecli-interactive
az group create --name myResourceGroup --location southeastasia 
```

## <a name="create-a-storage-account"></a>저장소 계정 만들기

이 샘플은 Azure Storage 계정의 blob 컨테이너에 이미지를 업로드합니다. 저장소 계정은 Azure Storage 데이터 개체의 저장 및 액세스를 위한 고유한 네임스페이스를 제공합니다. [az storage account create](/cli/azure/storage/account) 명령을 사용하여 만든 리소스 그룹에 저장소 계정을 만듭니다.

> [!IMPORTANT]
> 자습서의 2부에서는 Blob 스토리지에서 Azure Event Grid를 사용합니다. 스토리지 계정은 Event Grid를 지원하는 Azure 지역에 만듭니다. 지원되는 지역 목록은 [지역별 Azure 제품](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all)을 참조하세요.

다음 명령에서 `<blob_storage_account>` 자리 표시자가 표시되는 Blob Storage 계정에 대해 글로벌로 고유한 이름으로 바꿉니다.  

```azurecli-interactive
blobStorageAccount=<blob_storage_account>

az storage account create --name $blobStorageAccount \
--location southeastasia --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
```

## <a name="create-blob-storage-containers"></a>Blob 스토리지 컨테이너 만들기

앱은 Blob Storage 계정에서 두 컨테이너를 사용합니다. 컨테이너는 폴더와 비슷하며 Blob을 저장합니다. *images* 컨테이너는 앱이 고해상도 이미지를 업로드하는 위치입니다. 시리즈의 뒷부분에서 Azure 함수 앱은 크기가 조정된 이미지 썸네일을 *썸네일* 컨테이너에 업로드합니다.

[az storage account keys list](/cli/azure/storage/account/keys) 명령을 사용하여 저장소 계정 키를 가져옵니다. 그런 다음, 이 키를 사용하여 [az storage container create](/cli/azure/storage/container) 명령으로 두 개의 컨테이너를 만듭니다.  

*images* 컨테이너의 공용 액세스는 `off`로 설정되고, *thumbnails* 컨테이너의 공용 액세스는 `container`로 설정됩니다. `container` 공용 액세스 설정을 사용하면 웹 페이지를 방문하는 사용자가 썸네일을 볼 수 있습니다.

```azurecli-interactive
blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv)

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off

az storage container create -n thumbnails --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your Blob storage account key..."
echo $blobStorageAccountKey
```

Blob 스토리지 계정 이름과 키를 적어 두세요. 샘플 앱에서 이러한 설정을 통해 스토리지 계정에 연결하여 이미지를 업로드합니다. 

## <a name="create-an-app-service-plan"></a>App Service 플랜 만들기

[App Service 계획](../../app-service/overview-hosting-plans.md)은 위치, 크기 및 앱을 호스트하는 웹 서버 팜의 기능을 지정합니다.

[az appservice plan create](/cli/azure/appservice/plan) 명령으로 App Service 계획을 만듭니다.

다음 예에서는 **체험** 가격 책정 계층에서 `myAppServicePlan`이라는 App Service 계획을 만듭니다.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku Free
```

## <a name="create-a-web-app"></a>웹앱 만들기

웹앱은 GitHub 샘플 리포지토리에서 배포되는 샘플 앱 코드에 대한 호스팅 공간을 제공합니다. [az webapp create](/cli/azure/webapp) 명령을 사용하여 `myAppServicePlan` App Service 계획에 [웹앱](../../app-service/overview.md)을 만듭니다.  

다음 명령에서 `<web_app>`을 고유한 이름으로 바꿉니다. 유효한 문자는 `a-z`, `0-9` 및 `-`입니다. `<web_app>`이 고유하지 않으면 다음 오류 메시지가 표시됩니다. _이름이 `<web_app>`인 웹 사이트가 이미 있습니다._ 웹앱의 기본 URL은 `https://<web_app>.azurewebsites.net`입니다.  

```azurecli-interactive
webapp=<web_app>

az webapp create --name $webapp --resource-group myResourceGroup --plan myAppServicePlan
```

## <a name="deploy-the-sample-app-from-the-github-repository"></a>GitHub 리포지토리에서 샘플 앱 배포

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

App Service는 웹앱에 콘텐츠를 배포하는 여러 가지 방법을 지원합니다. 이 자습서에서는 [공용 GitHub 샘플 리포지토리](https://github.com/Azure-Samples/storage-blob-upload-from-webapp)에서 웹앱을 배포합니다. [az webapp deployment source config](/cli/azure/webapp/deployment/source) 명령을 사용하여 웹앱에 대한 Git 배포를 구성합니다.

샘플 프로젝트에는 [ASP.NET MVC](https://www.asp.net/mvc) 앱이 포함되어 있습니다. 이 앱은 이미지를 받아들이고, 스토리지 계정에 저장하고, 썸네일 이미지 컨테이너에서 이미지를 표시합니다. 웹앱은 Azure Storage 클라이언트 라이브러리의 [Microsoft.Azure.Storage](/dotnet/api/overview/azure/storage), [Microsoft.Azure.Storage.Blob](/dotnet/api/microsoft.azure.storage.blob) 및 Microsoft.Azure.Storage.Auth 네임스페이스를 사용하여 Azure 스토리지와 상호 작용합니다.

```azurecli-interactive
az webapp deployment source config --name $webapp \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
```

# <a name="nodejs-v2-sdktabnodejs"></a>[Node.js V2 SDK](#tab/nodejs)
App Service는 웹앱에 콘텐츠를 배포하는 여러 가지 방법을 지원합니다. 이 자습서에서는 [공용 GitHub 샘플 리포지토리](https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node)에서 웹앱을 배포합니다. [az webapp deployment source config](/cli/azure/webapp/deployment/source) 명령을 사용하여 웹앱에 대한 Git 배포를 구성합니다. 

```azurecli-interactive
az webapp deployment source config --name $webapp \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node
```

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)
App Service는 웹앱에 콘텐츠를 배포하는 여러 가지 방법을 지원합니다. 이 자습서에서는 [공용 GitHub 샘플 리포지토리](https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node-v10)에서 웹앱을 배포합니다. [az webapp deployment source config](/cli/azure/webapp/deployment/source) 명령을 사용하여 웹앱에 대한 Git 배포를 구성합니다. 

```azurecli-interactive
az webapp deployment source config --name $webapp \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node-v10
```

---

## <a name="configure-web-app-settings"></a>웹앱 설정 구성

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

샘플 웹앱은 [Azure Storage 클라이언트 라이브러리](/dotnet/api/overview/azure/storage)를 사용하여 이미지 업로드에 사용되는 액세스 토큰을 요청합니다. Storage SDK에서 사용되는 스토리지 계정 자격 증명은 웹앱에 대한 앱 설정에 설정됩니다. [az webapp config appsettings set](/cli/azure/webapp/config/appsettings) 명령을 사용하여 배포된 앱에 앱 설정을 추가합니다.

```azurecli-interactive
az webapp config appsettings set --name $webapp --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=$blobStorageAccount \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbnails \
AzureStorageConfig__AccountKey=$blobStorageAccountKey  
```

# <a name="nodejs-v2-sdktabnodejs"></a>[Node.js V2 SDK](#tab/nodejs)

샘플 웹앱은 [Azure Storage 클라이언트 라이브러리](https://docs.microsoft.com/javascript/api/azure-storage)를 사용하여 이미지 업로드에 사용되는 액세스 토큰을 요청합니다. Storage SDK에서 사용되는 스토리지 계정 자격 증명은 웹앱에 대한 앱 설정에 설정됩니다. [az webapp config appsettings set](/cli/azure/webapp/config/appsettings) 명령을 사용하여 배포된 앱에 앱 설정을 추가합니다.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string --resource-group $resourceGroupName \
--name $blobStorageAccount --query connectionString --output tsv)

az webapp config appsettings set --name $webapp --resource-group myResourceGroup \
--settings AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbnails \
AzureStorageConfig__AccountName=$blobStorageAccount \
AzureStorageConfig__AccountKey=$blobStorageAccountKey \
AZURE_STORAGE_CONNECTION_STRING=$storageConnectionString
```

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)

샘플 웹앱은 [Azure Storage 클라이언트 라이브러리](https://github.com/Azure/azure-storage-js)를 사용하여 이미지 업로드에 사용되는 액세스 토큰을 요청합니다. Storage SDK에서 사용되는 스토리지 계정 자격 증명은 웹앱에 대한 앱 설정에 설정됩니다. [az webapp config appsettings set](/cli/azure/webapp/config/appsettings) 명령을 사용하여 배포된 앱에 앱 설정을 추가합니다.

```azurecli-interactive
az webapp config appsettings set --name $webapp --resource-group myResourceGroup \
--settings AZURE_STORAGE_ACCOUNT_NAME=$blobStorageAccount \
AZURE_STORAGE_ACCOUNT_ACCESS_KEY=$blobStorageAccountKey
```

---

웹앱이 배포되고 구성되면 앱에서 이미지 업로드 기능을 테스트할 수 있습니다.

## <a name="upload-an-image"></a>이미지 업로드

웹앱을 테스트하려면 게시된 앱의 URL로 이동합니다. 웹앱의 기본 URL은 `https://<web_app>.azurewebsites.net`입니다.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

**사진 업로드** 지역을 선택하여 파일을 선택 및 업로드하거나 파일을 해당 지역으로 끌어갑니다. 성공적으로 업로드되면 이미지가 사라집니다. **생성된 썸네일** 섹션은 이 항목의 뒷부분에서 테스트할 때까지 비어 있습니다.

![.NET 사진 업로드](media/storage-upload-process-images/figure1.png)

샘플 코드에서 *Storagehelper.cs* 파일의 `UploadFiletoStorage` 작업은 [UploadFromStreamAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.uploadfromstreamasync) 메서드를 통해 이미지를 스토리지 계정 내의 *images* 컨테이너에 업로드하는 데 사용됩니다. 다음 코드 샘플에는 `UploadFiletoStorage` 작업이 포함되어 있습니다.

```csharp
public static async Task<bool> UploadFileToStorage(Stream fileStream, string fileName, AzureStorageConfig _storageConfig)
{
    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the blob container by passing the name by reading the value from the configuration (appsettings.json)
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ImageContainer);

    // Get the reference to the block blob from the container
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

    // Upload the file
    await blockBlob.UploadFromStreamAsync(fileStream);

    return await Task.FromResult(true);
}
```

다음 클래스 및 메서드는 이전 작업에서 사용됩니다.

|클래스  |방법  |
|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.azure.cosmos.table.storagecredentials)     |         |
|[CloudStorageAccount](/dotnet/api/microsoft.azure.cosmos.table.cloudstorageaccount)    |  [CreateCloudBlobClient](/dotnet/api/microsoft.azure.storage.blob.blobaccountextensions.createcloudblobclient)       |
|[CloudBlobClient](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient)     |[GetContainerReference](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getcontainerreference)         |
|[CloudBlobContainer](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer)    | [GetBlockBlobReference](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getblockblobreference)        |
|[CloudBlockBlob](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob)     | [UploadFromStreamAsync](/dotnet/api/microsoft.azure.storage.file.cloudfile.uploadfromstreamasync)        |

# <a name="nodejs-v2-sdktabnodejs"></a>[Node.js V2 SDK](#tab/nodejs)

**파일 선택**을 선택하여 파일을 선택한 다음, **이미지 업로드**를 클릭합니다. **생성된 썸네일** 섹션은 이 항목의 뒷부분에서 테스트할 때까지 비어 있습니다. 

![Node.js V2 사진 업로드](media/storage-upload-process-images/upload-app-nodejs.png)

샘플 코드에서 `post` 경로는 이미지를 BLOB 컨테이너에 업로드하는 일을 담당합니다. 이 경로는 모듈을 사용하여 업로드를 처리하는 데 유용합니다.

- [multer](https://github.com/expressjs/multer)는 경로 처리기에 대한 업로드 전략을 구현합니다.
- [into-stream](https://github.com/sindresorhus/into-stream)은 [createBlockBlobFromStream](https://azure.github.io/azure-sdk-for-node/azure-storage-legacy/latest/BlobService.html) 에서 요구하는 대로 버퍼를 스트림으로 변환합니다.

파일이 경로로 전송되면 파일이 Blob 컨테이너에 업로드될 때까지 파일의 내용이 메모리에 유지됩니다.

> [!IMPORTANT]
> 큰 파일을 메모리에 로드하면 웹앱의 성능에 부정적인 영향을 미칠 수 있습니다. 사용자가 큰 파일을 게시한다고 예상되는 경우 웹 서버 파일 시스템에서 파일을 준비한 다음, Blob 스토리지에 업로드하도록 예약하는 것이 좋습니다. 파일이 Blob 스토리지에 있으면 서버 파일 시스템에서 해당 파일을 제거할 수 있습니다.

```javascript
const
      express = require('express')
    , router = express.Router()

    , multer = require('multer')
    , inMemoryStorage = multer.memoryStorage()
    , uploadStrategy = multer({ storage: inMemoryStorage }).single('image')

    , azureStorage = require('azure-storage')
    , blobService = azureStorage.createBlobService()

    , getStream = require('into-stream')
    , containerName = 'images'
;

const handleError = (err, res) => {
    res.status(500);
    res.render('error', { error: err });
};

const getBlobName = originalName => {
    const identifier = Math.random().toString().replace(/0\./, ''); // remove "0." from start of string
    return `${originalName}-${identifier}`;
};

router.post('/', uploadStrategy, (req, res) => {

    const
          blobName = getBlobName(req.file.originalname)
        , stream = getStream(req.file.buffer)
        , streamLength = req.file.buffer.length
    ;

    blobService.createBlockBlobFromStream(containerName, blobName, stream, streamLength, err => {

        if(err) {
            handleError(err);
            return;
        }

        res.render('success', { 
            message: 'File uploaded to Azure Blob storage.' 
        });
    });
});
```

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)

**파일 선택**을 선택하여 파일을 선택한 다음, **이미지 업로드**를 클릭합니다. **생성된 썸네일** 섹션은 이 항목의 뒷부분에서 테스트할 때까지 비어 있습니다. 

![Node.js V10 사진 업로드](media/storage-upload-process-images/upload-app-nodejs.png)

샘플 코드에서 `post` 경로는 이미지를 BLOB 컨테이너에 업로드하는 일을 담당합니다. 이 경로는 모듈을 사용하여 업로드를 처리하는 데 유용합니다.

- [multer](https://github.com/expressjs/multer)는 경로 처리기에 대한 업로드 전략을 구현합니다.
- [into-stream](https://github.com/sindresorhus/into-stream)은 [createBlockBlobFromStream](https://azure.github.io/azure-sdk-for-node/azure-storage-legacy/latest/BlobService.html)에서 요구하는 대로 버퍼를 스트림으로 변환합니다.

파일이 경로로 전송되면 파일이 Blob 컨테이너에 업로드될 때까지 파일의 내용이 메모리에 유지됩니다.

> [!IMPORTANT]
> 큰 파일을 메모리에 로드하면 웹앱의 성능에 부정적인 영향을 미칠 수 있습니다. 사용자가 큰 파일을 게시한다고 예상되는 경우 웹 서버 파일 시스템에서 파일을 준비한 다음, Blob 스토리지에 업로드하도록 예약하는 것이 좋습니다. 파일이 Blob 스토리지에 있으면 서버 파일 시스템에서 해당 파일을 제거할 수 있습니다.

```javascript
const {
  Aborter,
  BlobURL,
  BlockBlobURL,
  ContainerURL,
  ServiceURL,
  StorageURL,
  SharedKeyCredential,
  uploadStreamToBlockBlob
} = require('@azure/storage-blob');

const express = require('express');
const router = express.Router();
const multer = require('multer');
const inMemoryStorage = multer.memoryStorage();
const uploadStrategy = multer({ storage: inMemoryStorage }).single('image');
const getStream = require('into-stream');
const containerName = 'images';
const ONE_MEGABYTE = 1024 * 1024;
const uploadOptions = { bufferSize: 4 * ONE_MEGABYTE, maxBuffers: 20 };
const ONE_MINUTE = 60 * 1000;
const aborter = Aborter.timeout(30 * ONE_MINUTE);

const sharedKeyCredential = new SharedKeyCredential(
  process.env.AZURE_STORAGE_ACCOUNT_NAME,
  process.env.AZURE_STORAGE_ACCOUNT_ACCESS_KEY);
const pipeline = StorageURL.newPipeline(sharedKeyCredential);
const serviceURL = new ServiceURL(
  `https://${process.env.AZURE_STORAGE_ACCOUNT_NAME}.blob.core.windows.net`,
  pipeline
);

const getBlobName = originalName => {
  // Use a random number to generate a unique file name, 
  // removing "0." from the start of the string.
  const identifier = Math.random().toString().replace(/0\./, ''); 
  return `${identifier}-${originalName}`;
};

router.post('/', uploadStrategy, async (req, res) => {

    const blobName = getBlobName(req.file.originalname);
    const stream = getStream(req.file.buffer);
    const containerURL = ContainerURL.fromServiceURL(serviceURL, containerName);
    const blobURL = BlobURL.fromContainerURL(containerURL, blobName);
    const blockBlobURL = BlockBlobURL.fromBlobURL(blobURL);

    try {
      
      await uploadStreamToBlockBlob(aborter, stream,
        blockBlobURL, uploadOptions.bufferSize, uploadOptions.maxBuffers);

      res.render('success', { message: 'File uploaded to Azure Blob storage.' });   

    } catch (err) {

      res.render('error', { message: 'Something went wrong.' });

    }
});
```
---

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>저장소 계정에서 이미지가 표시되는지 확인합니다.

[Azure Portal](https://portal.azure.com)에 로그인합니다. 왼쪽 메뉴에서 **저장소 계정**을 선택하고 저장소 계정의 이름을 선택합니다. **Blob Service** 아래에서 **Blob**을 선택한 다음, **images** 컨테이너를 선택합니다.

컨테이너에 이미지가 표시되는지 확인합니다.

![Azure Portal 이미지 컨테이너 목록](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>썸네일 보기 테스트

썸네일 보기를 테스트하려면 이미지를 **thumbnails** 컨테이너에 업로드하여 앱에서 **thumbnails** 컨테이너를 읽을 수 있는지 확인합니다.

[Azure Portal](https://portal.azure.com)에 로그인합니다. 왼쪽 메뉴에서 **저장소 계정**을 선택하고 저장소 계정의 이름을 선택합니다. **Blob Service**에서 **Blob**을 선택한 다음, **thumbnails** 컨테이너를 선택합니다. **업로드**를 선택하여 **blob 업로드** 창을 엽니다.

파일 선택기를 사용하여 파일을 선택하고, **업로드**를 선택합니다.

앱으로 다시 이동하여 **썸네일** 컨테이너에 업로드된 이미지가 보이는지 확인합니다.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)
![새 이미지가 표시된 .NET 이미지 크기 조정 앱](media/storage-upload-process-images/figure2.png)

# <a name="nodejs-v2-sdktabnodejs"></a>[Node.js V2 SDK](#tab/nodejs)
![새 이미지가 표시된 Node.js V2 이미지 크기 조정 앱](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)
![새 이미지가 표시된 Node.js V10 이미지 크기 조정 앱](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

시리즈의 2부에서는 썸네일 이미지를 자동으로 만들므로 이 이미지가 필요하지 않습니다. Azure Portal의 **썸네일** 컨테이너에서 업로드한 이미지를 선택하고 **삭제**를 선택하여 이미지를 삭제합니다. 

CDN을 사용하도록 설정하여 Azure 스토리지 계정의 콘텐츠를 캐시할 수 있습니다. Azure 스토리지 계정으로 CDN을 사용하도록 설정하는 방법에 대한 자세한 내용은 [Azure CDN과 Azure 스토리지 계정 통합](../../cdn/cdn-create-a-storage-account-with-cdn.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

시리즈의 1부에서는 스토리지와 상호 작용하도록 웹앱을 구성하는 방법을 알아보았습니다.

Event Grid를 사용하여 이미지의 크기를 조정하는 Azure 함수를 트리거하는 방법을 알아보려면 시리즈의 2부로 계속 진행하세요.

> [!div class="nextstepaction"]
> [Event Grid를 사용하여 업로드된 이미지의 크기를 조정하도록 Azure 함수 트리거](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

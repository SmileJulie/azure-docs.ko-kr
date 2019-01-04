---
title: ML 파이프라인 만들기, 실행 및 추적
titleSuffix: Azure Machine Learning service
description: Python용 Azure Machine Learning SDK를 사용하여 기계 학습 파이프라인을 만들고 실행합니다.  파이프라인은 데이터 준비, 모델 학습, 모델 배포 및 유추와 같은 ML(기계 학습) 단계를 연결하는 워크플로를 만들고 관리하는 데 사용됩니다.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 8478b6760921f4641cd214b1ff19cae9757b6d7e
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269045"
---
# <a name="create-and-run-a-machine-learning-pipeline-using-azure-machine-learning-sdk"></a>Azure Machine Learning SDK를 사용하여 기계 학습 파이프라인 만들기 및 실행

이 문서에서는 [Azure Machine Learning SDK](https://aka.ms/aml-sdk)를 사용하여 [기계 학습 파이프라인](concept-ml-pipelines.md)을 만들고 게시, 실행 및 추적하는 방법을 설명합니다.  이 파이프라인은 다양한 기계 학습 단계를 연결하는 워크플로를 만들고 관리하는 데 도움이 됩니다. 데이터 준비 및 모델 학습과 같은 파이프라인의 각 단계(phase)에 하나 이상의 단계(step)를 포함할 수 있습니다.

만든 파이프라인은 Azure Machine Learning Service [작업 영역](how-to-manage-workspace.md)의 멤버에게 표시됩니다. 

파이프라인은 원격 컴퓨팅 대상을 해당 파이프라인과 관련된 중간 및 최종 데이터의 계산 및 스토리지에 사용합니다.  파이프라인은 지원되는 [Azure Storage](https://docs.microsoft.com/azure/storage/) 위치에서 데이터를 읽고 쓸 수 있습니다.

>[!Note]
>Azure 구독이 없는 경우 시작하기 전에 체험 계정을 만듭니다. [Azure Machine Learning Service의 평가판 또는 유료 버전](http://aka.ms/AMLFree)을 지금 사용해 보세요.

## <a name="prerequisites"></a>필수 조건

* [개발 환경을 구성](how-to-configure-environment.md)하여 Azure Machine Learning SDK를 설치합니다.

* 모든 파이프라인 리소스를 수용하는 [Azure Machine Learning 작업 영역](how-to-configure-environment.md#workspace)을 만듭니다. 

 ```python
 ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
 ```

## <a name="set-up-machine-learning-resources"></a>기계 학습 리소스 설정

파이프라인을 실행하는 데 필요한 리소스를 만듭니다.

* 파이프라인 단계에서 필요한 데이터에 액세스하는 데 사용되는 데이터 저장소를 설정합니다.

* 데이터 저장소에 상주하거나 여기서 액세스할 수 있는 데이터를 가리키도록 `DataReference` 개체를 구성합니다.

* 파이프라인 단계가 실행될 [컴퓨팅 대상](concept-azure-machine-learning-architecture.md#compute-target)을 설정합니다.

### <a name="set-up-a-datastore"></a>데이터 저장소 설정
데이터 저장소는 파이프라인에서 액세스할 데이터를 저장합니다.  각 작업 영역마다 기본 데이터 저장소가 있습니다. 추가 데이터 저장소를 등록할 수 있습니다. 

작업 영역을 만들 때 [Azure File Storage](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) 및 [Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)는 기본적으로 작업 영역에 연결됩니다.  Azure File Storage가 작업 영역의 “기본 데이터 저장소”이지만 데이터 저장소로 Blob Storage를 사용할 수도 있습니다.  [Azure Storage 옵션](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks)에 대해 자세히 알아보세요. 

```python
# Default datastore (Azure file storage)
def_data_store = ws.get_default_datastore() 

# The above call is equivalent to this 
def_data_store = Datastore(ws, "workspacefilestore")

# Get blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")
```

파이프라인에서 액세스할 수 있도록 데이터 파일 또는 디렉터리를 해당 데이터 저장소에 업로드합니다.  이 예제에서는 데이터 저장소의 Blob Storage 버전을 사용합니다.

```python
def_blob_store.upload_files(
    ["./data/20news.pkl"],
    target_path="20newsgroups", 
    overwrite=True)
```

파이프라인은 하나 이상의 단계로 구성됩니다.  단계는 컴퓨팅 대상에서 실행되는 단위입니다.  단계는 데이터 원본을 사용하고 “중간” 데이터를 생성할 수 있습니다. 단계는 모델, 모델과 종속 파일이 있는 디렉터리 또는 임시 데이터와 같은 데이터를 만들 수 있습니다.  그런 다음, 파이프라인의 다른 후속 단계에서 이 데이터를 사용할 수 있습니다.

### <a name="configure-data-reference"></a>데이터 참조 구성

파이프라인에서 단계의 입력으로 참조할 수 있는 데이터 원본을 방금 만들었습니다. 파이프라인에서 데이터 원본은 [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) 개체로 표시됩니다. `DataReference` 개체는 데이터 저장소에 상주하거나 여기서 액세스할 수 있는 데이터를 가리킵니다.

```python
blob_input_data = DataReference(
    datastore=def_blob_store,
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")
```

중간 데이터(또는 단계의 출력)는 [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) 개체로 표시됩니다. `output_data1`은 단계의 출력으로 생성되며 하나 이상 후속 단계의 입력으로 사용됩니다.  `PipelineData`는 단계 간에 데이터 종속성을 도입하고 파이프라인에 암시적 실행 순서를 만듭니다.

```python
output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

### <a name="set-up-compute"></a>컴퓨팅 설정

Azure Machine Learning에서 컴퓨팅(또는 컴퓨팅 대상)은 기계 학습 파이프라인에서 계산 단계를 수행하는 머신 또는 클러스터를 가리킵니다. 예를 들어 단계를 실행하기 위한 Azure Machine Learning 컴퓨팅을 만들 수 있습니다.

```python
compute_name = "aml-compute"
 if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target: ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                min_nodes = 1, 
                                                                max_nodes = 4)
     # create the compute target
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # Can poll for a minimum number of nodes and for a specific timeout. 
    # If no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

## <a name="construct-your-pipeline-steps"></a>파이프라인 단계 생성

이제 파이프라인 단계를 정의할 준비가 되었습니다. Azure Machine Learning SDK를 통해 사용할 수 있는 여러 가지 기본 제공 단계가 있습니다. 이러한 단계 중 가장 기본 단계는 지정된 컴퓨팅 대상에서 Python 스크립트를 실행하는 `PythonScriptStep`입니다.

```python
trainStep = PythonScriptStep(
    script_name="train.py",
    arguments=["--input", blob_input_data, "--output", processed_data1],
    inputs=[blob_input_data],
    outputs=[processed_data1],
    compute_target=compute_target,
    source_directory=project_folder
)
```

단계를 정의한 후 일부 또는 모든 단계를 사용하여 파이프라인을 빌드합니다.

>[!NOTE]
>단계를 정의하거나 파이프라인을 빌드할 때는 Azure Machine Learning Service에 파일 또는 데이터가 업로드되지 않습니다.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

## <a name="submit-the-pipeline"></a>파이프라인 제출

파이프라인을 제출하면 각 단계에 대한 종속성이 확인되고, 소스 디렉터리로 지정한 폴더의 스냅숏이 Azure Machine Learning Service에 업로드됩니다.  소스 디렉터리를 지정하지 않으면 현재 로컬 디렉터리가 업로드됩니다.

```python
# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
```

파이프라인을 처음 실행하는 경우 다음이 수행됩니다.

* 프로젝트 스냅숏이 작업 영역과 연결된 Blob Storage에서 컴퓨팅 대상으로 다운로드됩니다.
* 파이프라인의 각 단계에 해당하는 Docker 이미지가 빌드됩니다.
* 각 단계의 Docker 이미지가 컨테이너 레지스트리에서 컴퓨팅 대상으로 다운로드됩니다.
* 한 단계에서 `DataReference` 개체를 지정하면 데이터 저장소가 마운트됩니다. 탑재가 지원되지 않는 경우 데이터가 대신 컴퓨팅 대상에 복사됩니다.
* 단계 정의에 지정된 컴퓨팅 대상에서 단계가 실행됩니다. 
* 단계에서 지정한 로그, stdout, stderr, 메트릭, 출력 등의 아티팩트가 생성됩니다. 그런 다음, 이러한 아티팩트는 사용자의 기본 데이터 저장소에 업로드되어 보관됩니다.

![파이프라인으로 실험 실행](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

## <a name="publish-a-pipeline"></a>파이프라인 게시

파이프라인을 게시하여 나중에 다른 입력을 사용하여 실행할 수 있습니다. 이미 게시된 파이프라인의 REST 엔드포인트가 매개 변수를 허용하려면 파이프라인을 게시하기 전에 매개 변수화해야 합니다. 

1. 파이프라인 매개 변수를 만들려면 기본값으로 [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) 개체를 사용합니다.

 ```python
 pipeline_param = PipelineParameter(
     name="pipeline_arg", 
     default_value=10)
 ```

2. 다음과 같이 이 `PipelineParameter` 개체를 파이프라인의 단계에 대한 매개 변수로 추가합니다.

 ```python
 compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],    
     target=compute_target, 
     source_directory=project_folder)
 ```

3. 이 파이프라인을 게시하면 호출 시 매개 변수를 허용합니다.

```python
published_pipeline1 = pipeline1.publish(
    name="My_Published_Pipeline", 
    description="My Published Pipeline Description")
```

## <a name="run-a-published-pipeline"></a>게시된 파이프라인 실행

게시된 모든 파이프라인에는 비-Python 클라이언트 등의 외부 시스템에서 파이프라인 실행을 호출하는 REST 엔드포인트가 있습니다. 이 엔드포인트는 일괄 처리 채점 및 다시 학습 시나리오에서 “관리되는 반복 가능성” 방법을 제공합니다.

이전 파이프라인의 실행을 호출하려면 [AzureCliAuthentication 클래스](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py)에서 설명하는 Azure Active Directory 인증 헤더 토큰이 필요합니다.

```python
response = requests.post(published_pipeline1.endpoint, 
    headers=aad_token, 
    json={"ExperimentName": "My_Pipeline",
        "ParameterAssignments": {"pipeline_arg": 20}})
```
## <a name="view-results"></a>결과 보기

모든 파이프라인 및 해당 실행 정보 목록을 참조하세요.
1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.  

1. [작업 영역을 보고](how-to-manage-workspace.md#view-a-workspace) 파이프라인 목록을 찾습니다.
 ![기계 학습 파이프라인 목록](./media/how-to-create-your-first-pipeline/list_of_pipelines.png)
 
1. 특정 파이프라인을 선택하여 실행 결과를 확인합니다.

## <a name="next-steps"></a>다음 단계
- [GitHub의 Jupyter 노트북](https://aka.ms/aml-pipeline-readme)을 사용하여 기계 학습 파이프라인을 추가로 살펴봅니다.
- [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) 패키지 및 [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) 패키지에 대한 SDK 참조 도움말을 읽습니다.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
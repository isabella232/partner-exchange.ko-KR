---
title: AEP로 배치 데이터 가져오기
description: 배치 파일을 Experience Platform으로 가져오는 방법 알아보기
exl-id: 50576b67-b3ba-498e-86f6-7e1986b76985
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 0%

---

# AEP로 배치 데이터 가져오기

AEP는 플랫 파일(예: Parquet)의 프로필 데이터가 포함된 배치 파일 또는 의 알려진 스키마를 준수하는 데이터를 수집할 수 있습니다. [!UICONTROL 경험 데이터 모델] (XDM) 레지스트리.

AEP는 배치 파일을 사용하여 데이터를 수집할 수 있습니다. 허용되는 형식은 JSON, Parquet 및 CSV입니다.

이 문서에서는 다음 내용을 다룹니다.

* 일괄 처리 수집 사전 요구 사항
* 일괄 처리 수집 모범 사례 및 제한
* 일괄 처리를 만드는 방법
* 일괄 처리를 완료하는 방법
* 배치 상태를 확인하는 방법

다음 [Postman 컬렉션](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) 는 연결된 번호로 호출을 사용하여 문서 전체에서 참조됩니다. Postman 컬렉션 설치 및 사용에 대한 자세한 내용은 Github에서 확인할 수 있습니다 [추가 정보](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) 페이지를 가리키도록 업데이트하는 중입니다. 다음 샘플 데이터 세트도 있습니다. [충성도](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) 및 [프로필](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) 데이터.

이 자습서의 모든 호출에 대해 Postman 호출 폴더를 사용합니다. 4: 일괄 가져오기, 4a: PROFILE 데이터에 대한 일괄 가져오기 또는 4b: EVENT 데이터에 대한 일괄 가져오기.

## 일괄 처리 수집 사전 요구 사항

* 스키마를 정의하고 데이터 세트를 만듭니다.
* 데이터는 JSON, Parquet 또는 CSV 형식으로 지정해야 합니다.
* [플랫폼 인증](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md).
* 위에 연결된 인증 자습서에서 필수 헤더에 대한 값을 수집합니다.

## 일괄 처리 수집 모범 사례 및 제한

* 최대 배치 크기: 100GB
* 배치당 최대 파일 수: 1500
* 파일이 512MB보다 큰 경우 더 작은 청크로 분할해야 합니다. 자세한 내용은 [개발자 안내서](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
* 행당 최대 속성 또는 필드 수: 10,000
* 사용자당 분당 최대 배치 수: 138

## 일괄 처리 만들기

이 자습서에서는 JSON을 형식으로 사용합니다. 더 많은 형식 예제는 [개발자 안내서](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
JSON을 입력 형식으로 사용하여 배치를 만듭니다(데이터 세트 ID를 포함하고 데이터가 데이터 세트에 연결된 XDM 스키마를 준수하는지 확인).

```json
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
  -d '{
          "datasetId": "{DATASET_ID}",
           "inputFormat": {
                "format": "json"
           }
      }'
```

응답:

```json
{
    "id": "{BATCH_ID}",
    "imsOrg": "{IMS_ORG}",
    "updated": 0,
    "status": "loading",
    "created": 0,
    "relatedObjects": [
        {
            "type": "dataSet",
            "id": "{DATASET_ID}"
        }
    ],
    "version": "1.0.0",
    "tags": {},
    "createdUser": "{USER_ID}",
    "updatedUser": "{USER_ID}"
}
```

## 파일 업로드

이제 파일을 새로 만든 일괄 처리에 업로드할 수 있습니다(위의 응답에서 batch_id 사용).

```json
curl -X PUT "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.json" \
  -H "content-type: application/octet-stream" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}" \
  --data-binary "@{FILE_PATH_AND_NAME}.json"
```

>[!NOTE]
>
>API는 단일 부분 업로드만 지원합니다. 즉, 개별 호출과 함께 각 파일/마이크로 배치를 업로드해야 합니다. content-type이 application/octet-stream인지 확인합니다.

응답:

```
200 OK
```

## 일괄 처리 완료

모든 파일이 업로드되면 이 호출은 배치가 승격할 준비가 되었음을 나타냅니다.

```json
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

응답:

```
200 OK
```

## 일괄 처리 상태 확인

배치 상태는 UI에서 또는 API를 통해 확인할 수 있습니다(아래 호출 참조). UI를 체크 인하려면 데이터 세트로 이동하여 상태를 확인합니다.

다양한 일괄 처리 수집 상태를 찾을 수 있습니다 [여기](https://adobe.ly/2TMMCmj).


```json
curl GET "https://platform.adobe.io/data/foundation/catalog/batch/{BATCH_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key: {API_KEY}"
```

응답:

```json
{
    "{BATCH_ID}": {
        "imsOrg": "{IMS_ORG}",
        "created": 1494349962314,
        "createdClient": "MCDPCatalogService",
        "createdUser": "{USER_ID}",
        "updatedUser": "{USER_ID}",
        "updated": 1494349963467,
        "externalId": "{EXTERNAL_ID}",
        "status": "success",
        "errors": [
            {
                "code": "err-1494349963436"
            }
        ],
        "version": "1.0.3",
        "availableDates": {
            "startDate": 1337,
            "endDate": 4000
        },
        "relatedObjects": [
            {
                "type": "batch",
                "id": "foo_batch"
            },
            {
                "type": "connection",
                "id": "foo_connection"
            },
            {
                "type": "connector",
                "id": "foo_connector"
            },
            {
                "type": "dataSet",
                "id": "foo_dataSet"
            },
            {
                "type": "dataSetView",
                "id": "foo_dataSetView"
            },
            {
                "type": "dataSetFile",
                "id": "foo_dataSetFile"
            },
            {
                "type": "expressionBlock",
                "id": "foo_expressionBlock"
            },
            {
                "type": "service",
                "id": "foo_service"
            },
            {
                "type": "serviceDefinition",
                "id": "foo_serviceDefinition"
            }
        ],
        "metrics": {
            "foo": 1337
        },
        "tags": {
            "foo_bar": [
                "stuff"
            ],
            "bar_foo": [
                "woo",
                "baz"
            ],
            "foo/bar/foo-bar": [
                "weehaw",
                "wee:haw"
            ]
        },
        "inputFormat": {
            "format": "parquet",
            "delimiter": ".",
            "quote": "`",
            "escape": "\\",
            "nullMarker": "",
            "header": "true",
            "charset": "UTF-8"
        }
    }
}
```

## 참조 문서

* [데이터 수집 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [일괄 처리 수집 개요](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/ingest_architectural_overview.md)
* [일괄 처리 수집 개발자 안내서](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
* [일괄 처리 수집 문제 해결 안내서](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_troubleshooting_guide.md)
* [데이터 수집 Postman 수집](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Data%20Ingestion%20API.postman_collection.json)
* [인증 자습서](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md)

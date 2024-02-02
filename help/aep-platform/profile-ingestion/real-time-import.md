---
title: 실시간 가져오기
description: 실시간으로 AEP로 데이터를 가져오는 방법을 알아봅니다.
exl-id: 0b6215a9-1160-49ae-8aa5-302b47357200
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# AEP로 데이터 스트리밍

Adobe [!DNL Experience Platform] 를 사용하면 프로필 및 경험 이벤트를 거의 실시간으로 스트리밍하고 사용할 수 있습니다. 스트리밍을 통해 AEP로 전송되는 모든 데이터는 데이터 레이크에서 유지됩니다. 데이터는 API를 통해 또는 Adobe Launch를 사용하여 기존 데이터 세트 또는 완전히 새로운 데이터 세트로 스트리밍할 수 있습니다.

이 문서에서는 다음 내용을 다룹니다.

* XDM 개별 프로필로 스트리밍
* XDM ExperienceEvent로 스트리밍
* AEP를 사용하여 Launch 확장을 스트리밍합니다.

다음 [Postman 컬렉션](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) 는 연결된 번호로 호출을 사용하여 문서 전체에서 참조됩니다. Postman 컬렉션 설치 및 사용에 대한 자세한 내용은 Github에서 확인할 수 있습니다 [추가 정보](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) 페이지를 가리키도록 업데이트하는 중입니다. 다음 샘플 데이터 세트도 있습니다. [충성도](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) 및 [프로필](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) 데이터.

## 전제 조건

* [플랫폼 인증](https://docs.adobe.com/content/help/en/experience-platform/tutorials/authentication.html).
* 위에 연결된 인증 자습서에서 필수 헤더에 대한 값을 수집합니다.

## 스트리밍 연결 만들기

AEP로 스트리밍하려면 먼저 스트리밍 연결을 만들어야 합니다. 스트리밍 연결에는 스트리밍 데이터의 소스 및 다음에 속한 레코드에서 전송 중인지 여부와 같은 속성이 포함됩니다. [!DNL Experience Data Model] (XDM) 스키마. 스트리밍 연결을 만들면 데이터를 AEP로 스트리밍하는 데 사용하는 고유한 URL이 제공됩니다.

이동 [여기](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/create-streaming-connection.html) api 또는 를 통해 스트리밍 연결을 만드는 방법에 대한 지침 [여기](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/create-streaming-connection-ui.html) UI를 통해 스트리밍 연결을 만드는 방법에 대한 지침을 제공합니다.

```json
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "Sample name",
     "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
     "description": "Sample description",
     "connectionSpec": {
         "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
         "version": "1.0"
     },
     "auth": {
         "specName": "Streaming Connection",
         "params": {
             "sourceId": "Sample connection",
             "dataType": "xdm",
             "name": "Sample connection"
         }
     }
 }
```

응답:

```json
 {
    "id": "77a05521-91d6-451c-a055-2191d6851c34",
    "etag": "\"a500e689-0000-0200-0000-5e31df730000\""
}
```

향후 스트리밍 수집 호출을 위해 위의 응답에 제공된 ID를 저장해야 합니다(Postman 컬렉션은 이 ID를 CONNECTION_ID 환경 변수에 저장합니다).

## AEP로 프로필 데이터 스트리밍

이 섹션의 경우 Postman 호출 폴더를 사용하십시오. 3: 실시간 가져오기, 3a: 프로필 데이터에 대한 실시간 가져오기.

스트리밍 프로필 데이터에 대한 응답과 함께 자세한 JSON 요청이 문서화되었습니다 [여기](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/streaming-record-data.html).

단계:

1. XDM 개별 프로필 스키마 만들기
1. XDM 개별 프로필에 대한 기본 ID 설명자 설정(기본 키)
1. XDM 개별 프로필 레코드에 대한 데이터 세트 만들기
1. 스트리밍 수집 API를 호출하여 XDM 개별 프로필 레코드 만들기
1. 새로 생성된 프로필 검색

## AEP로 경험 이벤트 스트리밍

이 섹션의 경우 Postman 호출 폴더를 사용하십시오. 3: 실시간 가져오기, 3b: 프로필 데이터에 대한 실시간 가져오기.

스트리밍 경험 데이터에 대한 응답과 함께 자세한 JSON 요청이 문서화되었습니다 [여기](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/streaming-time-series-data.html).

단계:

1. XDM ExperienceEvent 스키마 만들기
1. XDM ExperienceEvent(기본 키)에 대한 기본 ID 설명자 설정
1. XDM ExperienceEvents에 대한 데이터 세트 만들기
1. 스트리밍 수집 API를 호출하여 XDM ExperienceEvent를 만듭니다
1. 새로 생성된 이벤트 검색

## Experience Platform 태그를 사용하여 AEP로 스트리밍

Adobe [!DNL Experience Platform] Launch 확장은 Launch를 통해 AEP로 스트리밍하는 방법을 제공합니다. 자세한 내용은 다음을 참조하십시오. [이 안내서](https://docs.adobe.com/content/help/ko/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html).

## 참조 문서

* [데이터 수집 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [스트리밍 수집 개요](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/streaming_ingest_overview.md)
* [스트리밍 수집 개발자 안내서](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/getting_started_with_platform_streaming_ingestion.md)
* [AEP Launch Extension 사용](https://docs.adobe.com/content/help/ko/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html)

---
title: AEP 샌드박스 액세스 및 탐색
description: Experience Platform 샌드박스에 액세스하고 탐색하는 방법을 알아봅니다.
exl-id: 62c21615-4b03-4900-a1ad-8f809c836491
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '737'
ht-degree: 0%

---

# AEP 샌드박스 액세스 및 탐색

이 문서에서는 다음 사항을 다룹니다.

* 기존 Adobe Exchange Partner 샌드박스 조직과 공유 AEP 샌드박스 간의 차이점.
* AEP 공유 샌드박스에 대한 액세스 요청.
* AEP 공유 샌드박스에 대한 이메일 초대를 받는 중입니다.
* 에서 새 사용자 초대 [!DNL Admin Console].
* AEP UI 탐색.

AEP의 샌드박스 기술에 대한 일반적인 개요는 다음을 참조하십시오. [기사](https://docs.adobe.com/content/help/ko-KR/experience-platform/sandbox/home.html).

## 공유 AEP 샌드박스

Exchange 파트너는 다양한 Adobe에 액세스할 수 있습니다 [!DNL Experience Cloud] 제품 (과 같은 비 AEP 제품 [!DNL Analytics], [!DNL Target], Platform 태그 등) 자체 Adobe을 통해 [!DNL Experience Cloud] 조직(비공유) 파트너는 자체 조직에 대한 시스템 관리자 액세스 권한을 부여받아 사용자 및 기타 권한을 관리합니다. Adobe [!DNL Experience Platform] (AEP)는 다른 Adobe 샌드박스와 다르게 처리됩니다. 주요 차이점은 다음과 같습니다.

* AEP에 대한 액세스는 파트너 기본 Adobe을 통하지 않습니다. [!DNL Experience Cloud] 샌드박스 조직.
* AEP에 대한 액세스는 공유 Adobe Exchange 조직을 통해 이루어집니다.
* 다른 많은 Adobe Exchange 파트너 회사가 동일한 조직을 사용하여 AEP에 액세스하고 있습니다
   * AEP 샌드박스 기능을 통해 이 공유 조직 내의 데이터 및 활동을 다른 파트너가 보거나 수정할 수 없습니다. 각 파트너는 공유 조직 내의 다른 샌드박스에 액세스할 수 있습니다.
* 이 공유 조직 내의 관리 권한은 매우 제한적입니다.
* AEP의 샌드박스에 액세스할 수 있게 되면 파트너는 Admin Console 또는 기본 Experience Cloud 홈 페이지에 있는 동안 UI의 오른쪽 상단에 있는 조직 전환기에서 두 개의 조직을 볼 수 있습니다. 그러나 AEP에 로그인하면 공유 조직만 표시됩니다.

## 공유 AEP 샌드박스에 대한 액세스 요청

제출 [지원 요청](https://adobeexchangeec.zendesk.com/hc/en-us/requests/new) 다음 정보가 포함된 경우:

* Email Address
* 제목: AEP 샌드박스 요청
* 제품: 일반 프로비저닝/샌드박스
* 티켓 유형: 프로그램 지원 - Exchange 프로그램/프로비저닝 요청 질문
* 설명: AEP 샌드박스를 사용해야 하는 통합 사용 사례에 대한 간단한 설명을 제공합니다
* AEP 샌드박스에 추가해야 하는 모든 사용자 이름과 이메일도 제공해야 합니다. Adobe 후 사용자를 추가할 수 있지만 추가 티켓을 통해 요청을 통해 사용자를 추가해야 합니다(아래 참조).

## 이메일 초대 받기

AEP 샌드박스를 요청한 기본 담당자는 Adobe을 &quot;시작&quot;하도록 초대하는 자동화된 이메일을 받게 됩니다 [!DNL Experience Platform]. 기본 담당자는 다음 섹션에서 다루는 일부 관리 권한도 갖게 됩니다.

이메일에서 &quot;시작하기&quot; 버튼을 선택하는 대신 로 직접 이동합니다. `https://platform.adobe.com.` 초대에서 이메일 주소와 연결된 Adobe ID으로 로그인하거나 Adobe ID과 연결되지 않은 경우 만드십시오.

## 추가 사용자 초대

제출 [지원 요청](https://adobeexchangeec.zendesk.com/hc/en-us/requests/new) 다음 정보가 포함된 경우:

* 요청자 이메일 주소
* 제목: AEP 샌드박스 - 관리자/사용자 추가
* 제품: 일반 프로비저닝/샌드박스
* 티켓 유형: 프로그램 지원 - Exchange 프로그램/프로비저닝 요청 질문
* 설명: 추가할 사용자 목록(이름 및 이메일)

## AEP UI 탐색

AEP UI 보기 [소개 비디오](https://docs.adobe.com/content/help/en/platform-learn/tutorials/intro-to-platform/interface-tour.html)

AEP UI 내에는 왼쪽 패널을 통해 탐색할 수 있는 12개의 기본 영역이 있습니다. 그러나 이 유형의 통합에 가장 중요한 섹션은 스키마, 데이터 세트 및 프로필입니다.

* 홈 - 랜딩 화면

   * 몇 가지 시작 활동을 제안합니다.
   * 학습 콘텐츠에 대한 몇 가지 링크를 제공합니다.
   * 스키마, 데이터 세트 및 프로필과 같은 일부 기본 AEP 개체에 대한 대시보드 보기를 제공합니다

* 워크플로우 - AEP로 데이터를 가져오기 위한 일반 워크플로우로 실행
* 연결 / 소스 - AEP로 들어오는 데이터의 소스 관리
* 연결 / 대상 - 외부 시스템에 데이터를 보내기 위한 연결을 관리합니다.
* 프로필 - 개별 고객 프로필 보기 및 관리
* 세그먼트 - 고객 세그먼트 검색, 생성 및 수정
* ID - ID 네임스페이스를 검색, 생성 및 수정합니다. 고객을 고유하게 식별하는 데 사용되는 기본 ID 유형입니다
* 모델(데이터 과학) - 임베드된 Jupyter Notebooks 환경 사용을 비롯한 데이터 과학 활동에 참여합니다.
* 서비스(데이터 과학) - 데이터 과학 레서피를 서비스로 게시
* 스키마 - 스키마를 검색, 생성 및 수정합니다. 이는 데이터를 구성하는 데 사용되는 자세한 데이터 정의입니다
* 데이터 세트 - 데이터 세트를 검색, 생성 및 관리합니다. 데이터 세트는 스키마에 의해 정의되며 데이터가 AEP 내에 있는 위치입니다.
* 쿼리 - 데이터 세트 내의 데이터에서 통찰력을 얻기 위한 쿼리 저장소를 탐색, 생성, 수정 및 사용합니다.
* 모니터링 - 일괄 처리와 스트리밍 모두에 대해 AEP에서 들어오고 나가는 모든 데이터의 상태를 봅니다.

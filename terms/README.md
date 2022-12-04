## 📍 UTM parameter는 무엇일까?
언젠가 마케팅팀과 협업 할 때 `UTM parameter` 용어가 나왔는데, 해당 용어가 무엇인지 몰라 당황한 적이 있었다. 그래서 알아보자 `what is UTM parameter`?

`UTM(Urchin Tracking Module)`은 마케팅 담당자가 트래픽 소스 및 매체 등에서 온라인 마케팅 캠페인의 효과를 추적하는데 사용되며, 5개의 URL 매개변수(query string)로 사용된다. UTM 매개변수는 트래픽을 특정 웹사이트로 안내하는 캠페인을 식별하고 브라우저의 웹사이트 세션에 기여한다. 

여담으로 `Urchin`은 `Urchin Software Corporation`에서 개발한 웹 통계 분석 프로그램이고, 웹 서버 로그 파일 콘텐츠를 분석하고 로그 데이터를 기반으로 해당 웹사이트의 트래픽 정보를 표시했다. `Urchin Software Corp`는 2005년 4월 Google에 인수되어 지금의 `Google Analytics`가 되었다.

UTM의 매개변수는 5개가 있으며, `query` 사용 순서는 상관없다. 

| query | 목적 | 예시 |
| ----- | ----- | ----- |
| utm_source | 트래픽을 보낸 사이트를 식별하며 필수 매개변수입니다. | utm_source=google |
| utm_medium | 클릭당 비용 또는 이메일과 같이 사용된 링크 유형을 식별합니다. | utm_medium=service |
| utm_campaign | 특정 제품 프로모션 또는 전략적 캠페인을 식별합니다. | utm_campaign=air_reserve |
| utm_term | 검색어를 식별합니다. | utm_term=air_reserve_overseas |
| utm_content | 배너 광고 또는 텍스트 링크와 같이 사용자를 사이트로 유도하기 위해 구체적으로 클릭한 항목을 식별합니다. (A/B 테스트 및 콘텐츠 타겟 광고에 자주 사용됩니다.) | utm_content=로고링크 |

예시 URL은 다음과 같다. `https://google.com?utm_source=google&utm_medium=service&utm_campaign=air_reserve&utm_term=air_reserve_overseas&utm_content=로고링크`

`utm_medium`에서 `CPC`, `PPC`, `CPM`을 사용하는 경우가 있는데 `CPC(Cost Per Click)`는 노출과 상관없이 클릭 횟수 당 비용을 지불하는 방식이고, `PPC(Pay Per Click)`은 소비자가 광고주의 사이트를 방문할 경우 비용을 지불하는 방식이다. 마지막으로 `CPM(Cost Per Mile)`은 노출량을 기준으로 가격을 책정하는 방식을 의미한다. 예를 들어 100회 노출, 1000회 노출 등을 기준으로 가격 책정한다.

Reference
1. <a href='https://en.wikipedia.org/wiki/UTM_parameters'>UTM_parameters - WIKI</a>
2. <a href='https://en.wikipedia.org/wiki/Urchin_(software)'>Urchin - Wiki</a>
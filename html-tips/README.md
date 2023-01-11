## 📍 카카오톡 링크 공유시 og:description이 표시되지 않을 때
카카오톡에 링크 공유시 기존 openGraph와 다른 정보를 보여주기 위해 meta tag를 수정하는 작업을 하는데, `og:title`과 `og:description`을 넣었음에도 `og:description`이 보이지 않는 이슈가 있었다. 물론 <a href='https://developers.kakao.com/tool/debugger/sharing'>kakao-developers</a>사이트에서 URL 캐시 초기화를 진행했고 `og:url`도 내가 보여주려고 하는 `url`과 동일하게 설정했다. 결정적으로 `mobile`환경에서는 잘 나왔는데, `deskTop`에서는 잘 나오지 않았다.

여러번의 삽질 후 원인을 찾았는데 링크 공유시 openGraph의 `og:title`이 2줄이면 `og:description`이 보이지 않았다. (kakao developer Docs에 openGraph 관련 문서가 없어서 찾지 못했다.) 결론적으로 `og:title`을 1줄로 수정하니까 `og:description`이 정상적으로 나왔다.

Reference
1. <a href='https://kakaoad.github.io/kakao-pixel/catalog-guide.html'>OG 태그로 연동하기 - kakaoad.github.io</a>
2. <a href='https://devtalk.kakao.com/t/description/117076'>관련 문의 글1- devtalk.kakao</a>
3. <a href='https://devtalk.kakao.com/t/og-tag/103042'>관련 문의 글2- devtalk.kakao</a>
4. <a href='https://developers.kakao.com/docs'>문서 - kakao developers</a>
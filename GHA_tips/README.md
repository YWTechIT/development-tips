## 📍 release-please failed: Error creating Pull Request: Resource not accessible by integration 뜰 때
GHA로 PR을 자동화시키려는데 GHA에서 다음 에러를 마주했다.

![](https://res.cloudinary.com/ywtechit/image/upload/v1677666999/e5a4qb58nt6mhpyccuom.png)

원인은 간단한데 PR을 만드는 주체가 저장소에 대한 권한이 없기 때문에 발생했다. 그래서 Repo-settings를 들여다보니 Actions-General에 Workflow permissions 관련 설정이 있었다.

기존에는 `Read repository contents and packages permissions`에 체크되어 있었는데 `Read and write permissions`로 바꾸니 해결되었다.

![](https://res.cloudinary.com/ywtechit/image/upload/v1677667055/tidjorzeilqqgoabolas.png)

#### Reference
- <a href='https://github.com/actions/first-interaction/issues/10#issuecomment-545576314'>actions/first-interaction - issues#10</a>
 

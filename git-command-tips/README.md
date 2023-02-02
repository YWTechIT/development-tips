## 📍 pathspec (branchName) did not math any file(s) known to git
local 환경에서 origin branch를 불러오기 위해 `git checkout <branchName>` 혹은 `git switch <branchName>` 명령어를 입력하는데 다음과 같은 에러문구를 마주했다. 

![](https://res.cloudinary.com/ywtechit/image/upload/v1671693521/t49gnpbwsvcbms72e1gm.png)

이럴 땐 `origin`에 해당 브랜치가 있는지 확인하고, 원격 저장소의 최신 이력을 확인하는 명령어인 `git remote update`를 입력하고 `checkout`이나 `switch` 명령어를 입력하면 된다. 

Reference
- <a href='https://res.cloudinary.com/ywtechit/image/upload/v1671693521/t49gnpbwsvcbms72e1gm.png'>git-remote Docs</a>
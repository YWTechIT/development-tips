## 📍 알아두면 도움되는 npm commands
이번에 알아볼 npm 명령어는 `npm run ...`처럼 매번 사용되는 것은 아니지만, 마우스로 조작하기 번거로울 때 도움되는 명령어들이다.

1. `npm docs [package-name]`: 해당 패키지의 `docs`를 볼 수 있다. 예를 들어 `npm docs lodash`를 입력하면 <a href='https://lodash.com/'>lodash docs</a>를 볼 수 있다.(해당 패키지가 프로젝트에 설치되어 있지 않아도 볼 수 있다.)
2. `npm repo [package-name]`: 해당 패키지를 GitHub에서 볼 수 있다. 예를 들어 `npm repo lodash`를 입력하면 GitHub의 <a href='https://github.com/lodash/lodash'>lodash/lodash</a>페이지를 볼 수 있다. (해당 패키지가 프로젝트에 설치되어 있지 않아도 볼 수 있다.)
3. `npm outdated`: `package.json`에 있는 모든 패키지의 `current version`, `required version`, `latest version`을 볼 수 있다.

![](https://res.cloudinary.com/ywtechit/image/upload/v1670880963/ywtechit/u5r1u1rs1yrosafphh0t.png)

4. `npm v [package-name] versions`: 해당 패키지의 모든 version을 볼 수 있다. 예를 들어 `npm v typescript versions`를 입력하면 npm 페이지의 <a href='https://www.npmjs.com/package/typescript?activeTab=versions'>versions</a>에서 볼 수 있는 버전을 터미널에서 볼 수 있다.

![](https://res.cloudinary.com/ywtechit/image/upload/v1670881191/ywtechit/fyudvn5gaefuynhf6pqu.png)

5. `npm audit`: dependencies의 취약성을 확인하고, 취약점이 발견되면 적절한 대안을 알려준다. 취약점이 발견되지 않으면 `0 exit code`가 나온다. `npm audit fix` 혹은 `npm audit fix -f`로 취약점을 수정할 수 있다.  

![](https://res.cloudinary.com/ywtechit/image/upload/v1670881461/ywtechit/asenpdmqndxgqeywe5ha.png)

6. `npm v [package-name]`: 해당 패키지의 정보를 볼 수 있다.(`dist`, `dependencies`, `maintainers`, `dist tags` …)

![](https://res.cloudinary.com/ywtechit/image/upload/v1670881512/ywtechit/nbbm7rvdpuvypipaf6eg.png)

---
## 📍 filter + join 메서드로 가독성있는 코드 작성하기
`string | null` 타입을 가지는 `a`, `b`를 사용하여 `a·b`로 나타내야한다. 만약, 둘 중에 하나라도 `null`일 경우 `·`를 표시하지 않고, `string` 타입만 나타내야한다. 예를 들어 `a = 'foo'`이고, `b = bar`일 때 둘 다 값이 존재하면 `foo·bar`로, `a`타입만 존재하면 `foo`, `b`타입만 존재하면 `bar`, `a`와 `b` 둘 다 존재하지 않는 경우 `null | ''`을 나타내야한다. 이럴 때 어떻게 가독성있게 코드를 작성 할 것인가? 리액트에서 사용 할 때 `AS-IS`는 조건식과 `join` 메서드를 사용하여 `a && b ? [a, b].join('·') : a || b`로 나타냈는데, 가독성이 너무 떨어졌다. 그래서 `filter` 함수를 같이 사용하니 가독성이 이전보다 증가하였다. 

```typescript
const category: string | null = '개발';
const language: string | null = 'Typescript';

// AS-IS
const result1 =
  category && language ? [category, language].join('·') : category || language

// TO-BE
const result2 = [category, language].filter(Boolean).join('·')

// category, language 모두 값이 있는경우
👉🏾 result1: 개발·Typescript
👉🏾 result2: 개발·Typescript

// category는 존재하고 language는 null인 경우
👉🏾 result1: Typescript
👉🏾 result2: Typescript

// category는 null이고 language는 존재하는 경우
👉🏾 result1: 개발
👉🏾 result2: 개발

// 둘 다 null인 경우
👉🏾 result1: null
👉🏾 result2: ''
```

## 📍 shuffle 함수를 만들어보자
`JS`로 `shuffle` 함수를 만들 때 가장 먼저 생각나는 메서드는 `sort()` 일 것이다. 아마도 다음 코드처럼 구성하지 않았을까??

```javascript
// sort shuffle
const arr = [1, 2, 3, 4, 5];
const shuffle = arr.sort(() => Math.random() - 0.5)

console.log(shuffle);
👉🏾 [ 5, 3, 2, 1, 4 ]
```

구현 로직을 살펴보면 배열을 순회하면서 `Math.random()` 메서드를 사용하여 나온 랜덤한 값에 `-0.5`를 하여 `sort()` 메서드를 거치게 된다.  만약, `Math.random() - 0.5`를 한 값이 양수라면 오름차순으로 정렬되고, 음수를 리턴하면 내림차순으로 정렬된다. `arr` 배열을 모두 순회하게 되면 상단 코드블럭 5번 Line처럼 배열이 `shuffle`된 것을 알 수 있다. 

`sort` 함수로 구현했을 때의 장점은 코드가 간단해지는 반면, 단점은 모든 확률이 균일하게 일어나지 않는다. (JS 엔진마다 결과가 다를 수 있다.) 그 이유는 `sort`는 이런 용도로 만들어진 메서드가 아니기 때문이다. 이를 증명하기 위해 다음 코드를 살펴보자. `sort` 메서드를 이용해 배열 `[1, 2, 3]`을 백만번 `shuffle` 해봤다.

```javascript
const arr = [1, 2, 3];

function shuffle(array) {
  array.sort(() => Math.random() - 0.5);
}

let count = {
  '123': 0,
  '132': 0,
  '213': 0,
  '231': 0,
  '321': 0,
  '312': 0
};

for (let i = 0; i < 1_000_000; i++) {
  let array = [1, 2, 3];
  shuffle(array);
  count[array.join('')]++;
}

👉🏾 
123: 375285,
132: 62554,
213: 125270,
231: 62429,
312: 62557,
321: 311905
```

`count`를 살펴보면 빈도가 처음, 중간, 끝에 쏠려있는 것을 알 수 있다. 그 이유는 `sort` 메서드 때문인데, `sort`를 실행하면 인수로 넘긴 정렬 함수가 배열을 정리해주는데, 이 과정에서 `element` 비교가 완전 무작위로 이루어지기 때문이다. JS 엔진마다 내부 구현방식도 달라 이런 혼돈은 더 커지게 된다.

이를 해결하기 위해 `피셔-에이츠(Fisher-Yates)` 셔플을 이용하면 편향되지 않은 빈도로 `shuffle`을 할 수 있고, 시간복잡도도 `sort` 메소드보다 이점이 있다. Tim sort를 사용한 `sort` 메소드는 평균 `O(nlogn)`을 만족하지만, 피셔-에이츠는 `O(N)`을 만족하기 때문이다. `피셔-에이츠(Fisher-Yates)`의 구현순서는 다음과 같다.

1. 1부터 N까지의 수 중 임의의 숫자를 선택한다.
2. 1과 선택되지 않고 남아 있는 수 사이의 임의의 수 k를 선택한다.
3. k를 선택되지 않은 마지막 숫자와 자리를 바꾼다.
4. 1부터 N-1까지의 수 중 임의의 숫자를 선택한다. 
5. 더 이상 선택할 숫자가 없을 때까지 2~4번의 과정을 반복한다.

![](https://res.cloudinary.com/ywtechit/image/upload/v1667631041/yiqhdysdtinhtjwdzh6b.png)

이를 JS코드로 작성하면 다음과 같다.

```javascript
// Fisher-Yates shuffle
function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {  // 배열의 마지막부터 시작한다. i가 0이되면 반복문을 종료한다.
    let j = Math.floor(Math.random() * (i + 1));  // 랜덤한 값과 현재 인덱스 위치 + 1을 곱하고 소수점을 제거한다.
    [array[i], array[j]] = [array[j], array[i]];  // 현재 인덱스와 랜덤한값에 인덱스 위치 + 1을 곱하고 소수점을 제거한 값과 자리를 바꾼다.
  }
}
```

타입스크립트로 코드를 가독성 있게 작성해봤다. 그리고 백만번 `shuffle` 해봤고 결론적으로 확률이 균일하게 나오는 것을 알 수 있다.

```typescript
function shuffle<T>(array: T[]) {
  for (let index = array.length - 1; index > 0; index--) {
    const randomIndex = Math.floor(Math.random() * (index + 1))

    [array[index], array[randomIndex]] = [array[randomIndex], array[index]]
  }
  return array
}

let count = {
  '123': 0,
  '132': 0,
  '213': 0,
  '231': 0,
  '321': 0,
  '312': 0
};

for (let i = 0; i < 1000000; i++) {
  let array = [1, 2, 3];
  shuffle(array);
  count[array.join('')]++;
}

👉🏾
123: 166693
132: 166647
213: 166628
231: 167517
312: 166199
321: 166316
```

이렇게 `shuffle` 함수를 알아봤는데, 데이터의 크기가 작고 빈도에 상관없이 간편하게 `shuffle`을 구현하려면 `sort`를 사용해도 되지만, 데이터의 크기가 크고 발생 빈도가 균일하게 일어나야한다면 피셔-에이츠의 로직을 사용하는것이 어떨까?라고 생각하면서 글을 마친다.

Reference
1. <a href='https://ko.wikipedia.org/wiki/%ED%94%BC%EC%85%94-%EC%98%88%EC%9D%B4%EC%B8%A0_%EC%85%94%ED%94%8C'>피셔-에이츠 셔플</a>
2. <a href='https://bost.ocks.org/mike/shuffle/'>Fisher–Yates Shuffle - Mike Bostock</a>
3. <a href='https://www.youtube.com/watch?v=5sNGqsMpW1E'>How to shuffle an array in JavaScript - YouTube</a>

---
## 📍 window.history.pushState와 history.replaceState을 알아보자
`history.pushState()`는 브라우저의 세션 기록 스택에 새롭게 추가한다. 문법은 다음과 같은데, (`pushState(state, unused, url?)`) `state`는 사용자가 새로운 페이지로 이동할 때마다 `popstate` 이벤트가 발생한다. `unused`는 역사적 이유로 존재하며, 생략할 수 없다. 추후에 변경 예정이므로 당분간 빈 문자열('')을 사용하자. `url`을 사용하면 해당 `URL`로 이동하지 않지만, 나중에 사용자가 브라우저를 재 시작하면 해당 `URL` 로드를 시도할 수 있다. 반환 값은 `undefined`이다. 포인트는 `URL`값은 현재 `URL`과 동일한 출처(`same origin`)여야 하는데, 예를 들어 현재 페이지가 `www.google.com`인데, `history.pushState({ foo: 'bar' }, "", 'https://www.naver.com/')`을 사용 할 수 없다. 만약, 외부 URL을 호출하게 되면 다음과 같은 에러가 발생한다.

![](https://res.cloudinary.com/ywtechit/image/upload/v1666784121/ywtechit/y27k3f2jviyxvet3i3cs.png)

`history.pushState()`와 `window.location = "#foo"`은 현재 페이지의 `history`를 생성하는것은 같지만, `history.pushState()`의 이점(`advantage`)이 조금 더 크다.
1. `history.pushState()`에서 사용하는 `URL`은 `same origin`이다. 반면 `window.location`은 `hash`만 수정하는 경우에 동일한 페이지로 유지된다.
2. 세 번째 인자의 `URL`은 `optional`이지만, `window.location = "#foo"`와 같은 경우는 현재 `hash`값이 `#foo`가 아닌 경우에만 새로운 `history`를 만든다.
3. 임의의 데이터를 새 `history` 항목에 연결할 수 있다. `hash` 기반 접근 방식을 사용하면 모든 관련 데이터를 문자열로 인코딩해야 한다.
4. 새 URL이 이전 URL과 해시만 달라도 `pushState()`는 절대 `hashchange` 이벤트를 발생시키지 않는다.

`history.replaceState()`는 `history.pushState()`와 비슷하지만, 현 `depth`의 `session history`를 변경한다는 점에서 `history.pushState()`와 다르다. (`history.pushState()`는 `session history`에 새로운 세션 기록을 추가한다.) 문법은 `history.pushState()`와 동일하게 `replaceState(stateObj, un(used, url?)`로 작성한다. 반환 값은 `undefined`이다. 

예를 들어 현재 페이지(`https://www.mozilla.org/foo.html`)에서 새 탭에 다음을 입력해보자. `history.pushState({ foo: 'bar' }, '', 'bar.html');` 그런 후 콘솔창에 `history`를 입력하면 여전히 `length`가 1개인 것을 알 수 있다. 이를 통해 알 수 있는 점은 `history.pushState()`와 다르게 `session history`에 새로운 `history`를 추가한 것이 아니라 기존에 있던 `session history`값을 방금 선언한 값으로 대체한 것이다. 또 `history`의 `state`를 보면 기존에 없던 `state: { foo: 'bar' }`가 생긴 것을 알 수 있다. 그다음 `history.replaceState({ bar: 'baz' },'', 'bar2.html')`를 입력해보면 여전히 `length`가 1개이다. 알아두어야 할 점은 세 번째 인자인 `URL` 파라미터를 넣는다고 해도 브라우저는 `bar2.html`가 존재하는지 확인하지 않는다. 마지막으로 주소 표시창에 `www.naver.com`을 입력하여 엔터를 클릭하고 브라우저의 뒤로 가기 버튼을 클릭하면 마지막에 작성한 `bar2.html`이 나오고 한번 더 뒤로 가기를 누르면 `bar.html` 대신 `foo.html`가 나오고 `bar.html`을 완전히 우회한다는 점을 알 수 있다.

Reference
1. <a href='https://developer.mozilla.org/en-US/docs/Web/API/History/pushState'>History.pushState - MDN</a>
2. <a href='https://developer.mozilla.org/en-US/docs/Web/API/History/replaceState'>History.replaceState - MDN</a>
2. <a href='https://developer.mozilla.org/en-US/docs/Web/API/Window/popstate_event'>window: popstate event - MDN</a>

---
## 📍 npm install 후 package-lock.json의 diff가 많을 때
내가 담당한 `repository`를 작업하기 전에, 터미널에 `npm install`으로 해당 `project`를 실행하기 위해 필요한 패키지를 설치하는 도중 `VSCode`의 `Source Control - Changes`에서 `main` 브랜치의 `package-lock.json`의 코드와 다르게 `diff`가 엄청 많이 생기면서 `npm run dev`를 실행하면 오류가 뜨면서 정상적으로 `local` 환경 실행이 안되는 경우가 있었다.

![](https://velog.velcdn.com/images/abcd8637/post/1e2defd0-55ec-4213-bcbf-ba265974c984/image.png)

`main` 브랜치의 `package-lock.json`과 다른것도 없는데 갑자기 `diff`가 많이 생겨서 당황했지만, 침착을 유지하며 구글링을 한 결과 원인은 `npm`이 항상 `package-lock.json`에서 가능한 모든 데이터를 가져오려고 시도하는 특징 때문에 생긴 문제였다. 프로젝트 파일 중 `package-lock.json`을 잘 들여다보면 3번째 줄에 `lockfileVersion`이란 코드가 있는데, 이는 `document`의 체계적인 버전을 나타내는 코드이다. 

`lockfileVersion`은 `npm`의 버전에 따라 1과 2로 나뉜다. `npm v5` ~ `npm v6`는 1이 사용되고, `npm v7`이상은 2가 사용된다. (여담으로 `lockfileVersion` 버전 3도 있는데, 이는 `npm v7`이상에서 `node_modules/.package-lock.json`의 숨겨진 잠금 파일에 사용된다. 만약, 현재 패키지가 `npm v6`에 대한 지원이 없다면 향후 버전에서 사용 될 가능성이 높다.) 결론적으로 문제 원인은 `lockfileVersion`이 2에서 1로 낮아지면서 생긴 문제인데, 당시 나의 로컬 환경의 `npm` version은 `6.14.7`이었고, `node` version은 `14.8.0`이었다. (`npm` 버전을 확인하려면 `npm -v`를, `node`의 버전을 확인하려면 `node -v`를 입력하자.)

하지만, 이전까지 나는 `npm`의 버전을 강제로 낮춘적이 없었었는데 왜 `downgrade`가 됐는지 곰곰이 생각해보니 `legacy` 프로젝트를 실행시킬 때 최신 `node` version 지원이 안되어서 `nvm`으로 `node` version을 강제로 낮추었더니 해당 `node` version에 호환되는 `npm` version으로 바뀌었기 때문이다. 이후 삽질해보며 알게된 사실인데 `node-version`과 호환되는 `npm` 버전이 있었다. (Reference 2번 참고)

결론적으로 <a href='https://nodejs.org/ko/'>nodejs.org</a>에 접속하여 node.js에서 제공하는 안정적인 노드 버전인 `16.15.1 LTS`를 설치하고, 해당 노드버전과 호환되는 `npm` 버전인 `8.11.0`을 설치하고나서 `npm install`을 하니까 `package-lock.json`의 `diff`가 없이 정상적으로 실행되었다.

터미널 명령어는 `nvm ls -remote`로 지원되는 노드 버전을 확인하고, `nvm install 16.15.1`로 해당 노드 버전을 `nvm`에 설치하고,(`nvm`이 없다면 설치하는 방법을 구글에 찾아보자.) `npm use 16.15.1`로 노드 버전을 적용시키고, 마지막 `npm install -g npm@8.11.0`으로 `npm`의 버전을 맞춰주었다.

이렇게 또 하나의 문제를 해결했다. 혹시라도 나와 같은 문제로 고생하고 있는 다른 분들에게 도움이 되었으면 좋겠는 생각을 하며 글을 마친다.

Reference
1. https://docs.npmjs.com/cli/v8/configuring-npm/package-lock-json
2. https://nodejs.org/ko/download/releases/
3. https://nodejs.org/ko/

---
## 📍 npm과 caret, tilde, 패키지 설치하는 방법 알아보기
`npm`은 자바스크립트 패키지 매니저다. `Node.js`에서 사용할 수 있는 모듈들을 패키지화해서 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 `CLI(Command line Interface)`를 제공한다. 자신이 작성한 패키지를 공개할 수도 있고, 필요한 패키지를 검색하여 재사용할 수도 있다.

내가 다니는 회사에서는 `npm` 패키지를 직접 만들어 사용하고 관리한다.(<a href='https://www.npmjs.com/~triple-corp'>npm 패키지 보러가기</a>) 취업준비할 때 프로젝트를 하면 항상 `패키지 === 가져다 쓰는 것`이라고 생각했는데, 직접 패키지를 만들고 버전을 관리해야 하다보니 까다로운 것이 한둘이 아니었다. 그래서 패키지 버전의 기본적인 내용들을 현업에 적용할 때 까먹지 않기 위해 글을 남긴다. 우리가 프로젝트를 만질 때 `package.json`에 패키지를 설치하는데, 이때 패키지에는 다음처럼 버전이 명시되어있다.

```javascript
// package.json
"typescript": "3.8.3"
"lint-staged": "^8.1.1",
"nodemon": "~1.19.1",
```

예를 들어, `typescript`의 경우에는 `3.8.3`이라고 명시되어있는데 이러한 숫자를 `semantic versioning`이라고 부른다. 버전의 왼쪽부터 차례대로 주(主) 버전(Major), 부(部) 버전(Minor), 수(修) 버전(Patch)라고 부른다. `Major`는 기존 버전과 호환되지 않게 API가 바뀌면 변경되고, 기존 버전과 호환되면서 새로운 기능을 추가할 때는 `Minor`를 올리고, 기존 버전과 호환되면서 버그를 수정한 것이라면 `Patch`를 올린다. 

![](https://velog.velcdn.com/images/abcd8637/post/afbd85bf-3e75-42c9-bf81-9027b55147ee/image.jpeg)

상단 `codeBlock`에 패키지 버전을 보면 단순히 숫자만 붙여있는게 아니라 숫자 앞에 `^` 혹은 `~`가 붙어있는데, `^`는 `caret(캐럿)`이라 부르고, 물결표시인 `~`는 `tilde(틸드)`라고 부른다. 이들은 각각 특징이 있는데 `^`는 `Compatible with version`을 뜻하며 나중에 업데이트를 하더라도 `major`값이 변하지 않는 최대 버전을 설치하게 된다. 예를 들어 `"lint-staged": "^8.1.1"` 패키지를 업데이트 하게되면 `major`값이 변하지 않는 최신 버전인 `8.2.1`을 내려받게 된다. 이를 위해 간단한 실험을 진행했는데, `npm init -y`로 `package.json`을 만들고 `npm i lint-stages@8.1.1`로 `8.1.1`버전의 패키지를 다운받고나서 `npm i lint-stages`를 진행하니 `8.2.1`버전을 받았다. 이를 통해 패키지를 업데이트하게 되면 `^`은 해당 `major`를 제외한 버전의 최신 버전을 다운받는다는 점을 알 수 있었다. 

![](https://velog.velcdn.com/images/abcd8637/post/a6ce9f9b-1d34-447e-8d93-b4d94f782e94/image.png)

![](https://velog.velcdn.com/images/abcd8637/post/b4241f5c-ac4f-4461-a5ac-e50aeeb48938/image.png)

다음으로 `~`는 `tilde(틸드)`는 `Approximately equivalent to version`을 뜻하며, 패키지 버전 설치 명령어를 어디까지 입력했는지에 따라 다르다. 예를들어 `npm i nodemon@~1`이라고 작성하면 버전을 `major`까지만 명시했기 때문에 `minor`와 `patch` 변경을 허용한다. 그래서 하단 사진처럼 `major`는 1이고 나머지는 최신버전을 다운받는다. 이는 `1.x.x`와 같다.

![](https://velog.velcdn.com/images/abcd8637/post/7a425b18-09b1-4cee-956d-4c873cdbe3bf/image.png)

같은 방법으로 `npm i nodemon@~1.10`을 다운받으면 `patch`변경을 허용 할 것이고, 이는 `1.10.x`와 같다.

여담으로 `^` 혹은 `~`를 붙이지 않기 위해 `npm install nodemon@1.10.1`을 입력하고 `package.json`을 보면 자동으로 caret이 붙여있는데, 이는 `npm save-prefix`가 기본적으로 `^`으로 설정되어있기 때문이다. 만약, default값을 `^` 대신 `--save-exact`나 `~`로 바꾸고 싶다면 `npm config set save-prefix '--save-exact'` 혹은 `npm config set save-prefix '~'`으로 변경하면 된다. 변경된 내용은 `npm config set ls -l` 명령어에서 `save-prefix` 키 값에서도 확인 할 수 있다. 또한, `npm config`를 건들지않고 명령어를 사용하여 정확한 버전을 받고 싶다면 `npm i --save-exact <package>@<version>`을 입력하면 된다.

Reference
1. https://poiemaweb.com/nodejs-npm
2. https://docs.npmjs.com/about-semantic-versioning
3. https://semver.org/lang/ko/
4. https://stackoverflow.com/questions/22343224/whats-the-difference-between-tilde-and-caret-in-package-json
5. https://bytearcher.com/goodies/semantic-versioning-cheatsheet/

---
## 📍 Call by Value, Call by reference, Call by sharing

CS 공부를 하며 `Call by Value, Call by Reference`에 대해서 배우고 있는데, 헷갈리는 글들이 많았다. 결론적으로 `JS`에서는 `Call by reference, Call by sharing` 두 용어보다 `Call by Value` 용어를 사용하는 편이 알맞다. 다음 예시를 들어보자.

```javascript
// 원시값 변경시도
function foo(argument){
    argument = 10;
    console.log(argument)    // 10
}

// 이해하기 쉽게 작성한 함수
function foo(){
    let argument = 5;
    argument = 10;
    console.log(argument)    // 10
}

let argument = 5;
foo(argument);

console.log(argument);    // 5
```

원시값(Primitive type: `string` `boolean` `number` `null` `undefined`)은 주소 대신 값 자체를 복사하기 때문에 함수로 전달한 `argument`와 실제로 함수에서 받은 `argument`의 값은 다른 값이 된다. 때문에 첫번째 함수 내부의 argument `console.log`는 10이되고, 함수 밖에서의 argument `console.log`는 5가 된다. 

다음으로 원시값(Primitive type)이 아닌 객체(Object)의 속성을 변경하면 어떻게 될까?

```javascript
// 객체타입의 속성 변경
function foo(argument){
    argument.a = 5
    console.log(argument)    // { a : 5 }
}

// 이해하기 쉽게 작성한 함수
function foo(){
    let arguments = argument
    argument.a = 10
    console.log(arguments)    // { a : 5 }
}

let argument = {a: 5} 
foo(argument)
console.log(argument)    // { a : 5 }
```

객체타입의 인자는 주소값을 갖는 새로운 변수가 되는데, 이때는 동일한 객체를 바라보고있다 따라서 함수내부의 `argument`가 바뀌면 함수 외부의 `argument`도 같이 바뀐다. 즉 함수 내부의 값과 외부의 값이 함수 내부의 값으로 바뀐다.

그럼 마지막으로 다음은 어떻게 될까?

```javascript
function foo(argument){
    argument = 5;
    console.log(argument)    // ?
}

let argument = {a: 10}
foo(argument)
console.log(argument)    // ?
```

코드의 흐름을 살펴보면 이때는 함수로 넘어가는 `argument`는 객체, 함수 내부에서 `argument`는 원시 값으로 변경했다. 이때는 속성을 수정한것이 아닌 값 전체를 바꾸는것이기 때문에 함수내부는 `5`, 함수외부는 `{a : 10}`로 나오게된다. 만약, `c, c++`와 같이 `call by reference`가 존재하는 언어라면 값이 함수 외부의 값도 `5`로 바뀌었을 것이다. (대신 `call by sharing`이라고도 불리는데 이는 정식 명칭이 아니라는 말도 있어 사용하지 않았다.)

지금까지의 내용을 토대로 정리하면 `객체의 속성을 수정 할때는 참조 관계지만 값 자체를 수정하는 경우 그 관계가 깨진다`라고 결론을 내릴 수 있다. 또, 함수 내에서 원시값을 변경하는 경우 값은 바뀌지 않지만, 함수 내에서 객체의 속성을 변경하는 경우 값이 바뀐다.

### 📋 reference
1. <a href='https://www.youtube.com/watch?v=-w-oJp6OVd4&t=9s'>ZeroCho의 JS 중급 강좌 12-1. call by value, call by reference, call by sharing</a>
2. <a href='https://developer.mozilla.org/ko/docs/Glossary/Primitive'>MDN - 원시 값</a>
3. <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Working_with_Objects'>MDN - Working_with_Objects</a>

---
## 📍 Spread 연산자(...)의 활용
`TS(TypeScript)`를 공부하다가 `Spread`를 다양한 곳에 사용함을 깨닫고 정리하기 위해 글을 남긴다. 대체로 다음과 같은 상황에서 자주 쓰인다.

1. 객체 복사(주소 참조가 아닌 값 복사, `origin` 값 변경 X)
2. 객체 병합
3. 객체 특정 값 변경(`useState` 사용 시)
4. 잔여 연산자(rest operator) 사용

다음의 코드를 하나씩 살펴보자.

```javascript
// 1. 객체 복사

// * spread 연산자 미 사용(주소 참조, 본래의 값이 바뀐다.)
const myScore = [80, 85, 90];
const newScore = myScore
newScore[0] = 50
console.log(myScore)
👉🏽 [50, 85, 90]

// * spread 연산자 사용(값 참조, 본래의 값이 바뀌지 않는다.)
const myScore = [80, 85, 90];
const newScore = [...myScore];
newScore[0] = 50
console.log(myScore)
👉🏽 [80, 85, 90]

// * 객체 복사
const myInfo = {name: 'AYW', age:27,  marriage: false}
const newInfo = {...myInfo}
console.log(newInfo)
👉🏽 {name: 'AYW', age:27,  marriage: false}

// 2. 객체 병합
const firstInfo = {name: 'AYW'}
const secondInfo = {age: 27}
const resultInfo = {...firstInfo, ...secondInfo}
console.log(resultInfo)
👉🏽 {name: 'AYW', age:27}

// 3. 객체 특정 값 변경
const myInfo = {name: 'AYW', age:27,  marriage: false}
const newInfo = {...myInfo, marriage: !myInfo.marriage}
console.log(newInfo)
👉🏽 {name: 'AYW', age:27,  marriage: true}

// 4. 잔여 연산자(spread operator) 사용
const myInfos = {money: 3000, name: 'AYW', age:27, }
const {money, ...restInfo} = myInfos
console.log(restInfo)
👉🏽 {name: 'AYW', age:27}
```

---
## 📍 Class vs ProtoType
`JavaScript`는 `객체지향언어`이긴 하지만 `Class` 문법을 사용하지 않고 대신 `Prototype` 문법을 사용한다. (ES6 이후에 `class` 문법이 들어왔지만 이는 `JS`가 `class` 기반의 문법으로 바뀐것은 아니다.) 그렇다면 `class`처럼 사용하는 문법과 `prototype` 문법은 언제 사용할까? 다음 코드를 살펴보자.

```javascript
// use class
function myInfo() {
    this.name = 'AYW'
    this.age = 27
}

const myName = new myInfo();
const myAge = new myInfo();

console.log(myName.name);    // AYW
console.log(myAge.age);    // 27

// prototype
function myInfo() {}

myInfo.prototype.name = 'AYW';
myInfo.prototype.age = 27;

const myName = new myInfo();
const myAge = new myInfo();

console.log(myName.name);    // AYW
console.log(myAge.age);    // 27
```

차이점이 느껴지는가? `class` 문법의 단점은 `myInfo` 내에 있는 값 모두가 메모리에 할당된다는 것이다. 즉, `100개`의 객체를 선언하면 `200개`의 값이 메모리 변수에 올라가있다는 것이다. 만약, `a` 변수에는 `name`과 `age` 모두를 사용하지만, `b` 변수에는 `name`만 사용하고 싶지만 사용하지 않는 `age` 변수까지 메모리에 할당된다는 이야기다. 이는 곧 메모리의 효율과 관련이 되는데 이를 방지하기 위해 `prototype`을 사용한다. 

`prototype`은 함수를 지정할 때 `prototype`이라는 고유 속성 값을 지정해줌으로서 객체를 생성할 때마다 해당 객체는 이미 지정된 `prototype`에 속한 함수 값만 참조하면 되는것이다. 즉, `prototype`을 쓰면 200개의 변수가 메모리에 저장되는 것이아니라, 함수 값만 참조하므로 메모리가 절약되는 효과를 일으킬 수 있다.

`prototype` 코드의 흐름은 다음과 같다.
1. `prototype`의 빈 객체를 어딘가에 선언한다.
2. `myName`과 `myAge`는 `prototype`의 빈 객체 내에 존재하는 `Object`의 값을 모두 사용할 수 있다.
  * 쉽게 말해 `name`과 `age`를 `prototype`의 빈 객체 어딘가에 넣어두고 `myName`과 `myAge` 변수가 해당 값을 꺼내쓰는 형태라고 보면 된다.

지금까지 내용으로 보아 `prototype`은 직접 함수내부에 선언하지도 않은 값을 넣어둘 수 있다고 배웠다. 그럼 어떻게 값을 저장하고 접근한다는 말인가? 해답은 `prototype Object` 와 `prototype Link`에 있는데, 하나씩 차근차근 살펴보자. 

### 📍 ProtoType Object(constructor, __proto__)
객체는 언제나 `함수(Function)`로 생성된다. 함수로 정의되면 기본적으로 얻는 자격이 있는데 바로 `constructor(생성자)`다. 이것이 있기 때문에 우리는 `new`를 통해 객체를 만들 수 있는 것이다. 또, 함수를 생성하면 `prototype`의 속성과 `ProtoType Object`가 만들어지는데, 생성된 함수에 `prototype`의 속성을 통해 `ProtoType Object`에 접근할 수 있는 권한이 생긴다. `ProtoType Object`는 말 그대로 프로토타입 객체이고, 이해하기 쉽게 일반 객체라고 생각하면된다. 일반 객체에는 값을 자유롭게 추가하거나 삭제 할 수 있다. 

![](https://images.velog.io/images/abcd8637/post/cf3b7e9d-5255-4fea-a8f7-5df51d76597a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-15%2006.36.35.png)

![](https://images.velog.io/images/abcd8637/post/35babb3d-367a-4634-aa1f-30e1840384f9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-15%2006.39.12.png)

`prototype_object`는 `constructor(생성자)`와 `__proto__` 속성을 갖게 된다. `constructor(생성자)`는 생성 함수인 `function myInfo() {}`를 가리키고 있다. 그렇다면 `__proto__`는 뭘까? 우리가 함수 내부에 값을 선언하지 않았는데도 접근할 수 있는 이유가 바로 `__proto__`때문이다. `__proto__`는 객체가 생성될 때 `origin` 격의 함수인 `prototype object`를 가리킨다. 즉, `name`과 `age`는 `myInfo`로부터 생성되었으니 `myInfo` 함수의 `prototype object`를 가리키고 있는다. 이처럼 `__proto__` 속성을 통해 상위 프로토타입과 연결되어있는 형태를 `프로토타입 체인(prototype chain)`이라고 한다.

```javascript
function myInfo() {}    // 함수

myInfo.prototype.name = 'AYW'
myInfo.prototype.age = 27

let an = new myInfo();  // 함수를 객체로 생성

console.log(an.name) // 'AYW'
console.log(an.age)  // 27
```

![](https://images.velog.io/images/abcd8637/post/e66d7342-83ec-43bf-ae82-52e67dac16c2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-15%2006.43.09.png)

`prototype chain`의 또 다른 예를 한번 살펴보자.

```javascript
function Boss (){}
Boss.prototype.boolean = true;

function MiddleBoss (){}
MiddleBoss.prototype = new Boss();

function Worker(){}
Worker.prototype = new MiddleBoss();

let o = new Worker();
console.log(o.boolean)    // true
```

이 과정은 `Boss` -> `MiddleBoss` -> `Worker` 순으로 `boolean`값을 내려주는 코드이다. 이 코드가 어떻게 `o.boolean`값이 `true`가 나올 수 있을까?

1. 마지막 변수인 `o`의 `property` 중 `boolean` 값이 있는지 살펴보고 있으면 반환시킨다.
2. 값이 없으면 생성자를 추적한다. 상위 `prototype`인 `Worker`의 `property`에서 `Worker.prototype.boolean`을 찾고 있으면 반환시킨다.
3. 값이 없으면 생성자를 추적한다. 상위 `prototype`인 `MiddleBoss`의 `property`에서 `MiddleBoss.prototype.boolean` 찾고 있으면 반환시킨다.
4. 값이 없으면 생성자를 추적한다. 상위 `prototype`인 `Boss`의 `property`에서 `Boss.prototype.boolean` 찾고 있으면 반환시킨다.

이런 순서로 진행 될 것이다. 만약, 다음 코드는 값이 어떻게 나올까?

```javascript
function Boss (){}
Boss.prototype.boolean = true;

function MiddleBoss (){}
MiddleBoss.prototype = new Boss();

function Worker(){}
let t = new Worker();
t.boolean = false;
Worker.prototype. = t

let o = new Worker();
console.log(o.boolean)    // ??
```

정답은 `false`다. 순서는 다음과 같다.

1. 마지막 변수인 `o`의 `property` 중 `boolean` 값이 있는지 살펴보고 있으면 반환시킨다.
2. 값이 없으면 생성자를 추적한다. 상위 `Worker.prototype`를 살핀다.
3. `prototype`에 저장되어있는 `boolean` 값이 `4`이기 때문에 `4`를 반환한다.

reference
1. <a href='https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67'>javascript-prototype-이해하기</a>
2. <a href='http://insanehong.kr/post/javascript-prototype/'>Javascript 기초 - Object prototype 이해하기</a>
3. <a href='https://opentutorials.org/module/4047/24610'>생활코딩 - prototype</a>

---
### 📍 배열의 특정 인덱스를 제거하거나 특정 범위만 반환하는 함수, filter, splice, slice

알고리즘 문제를 풀다가 배열의 특정원소나 인덱스를 삭제하거나 특정 범위를 제거하고 싶은데 `MDN`을 찾아보니까 여러가지 함수가 존재했다. 상황마다 다르지만 로직을 구현 할 때 다음의 선택지를 보고 골라서 사용하면 된다.

원본 배열을 변경하고 싶지않고 특정 인덱스를 제거하고 싶을 때(새로운 변수에 선언하고 싶을 때)
1. <a href='https://developer.mozilla.org/ko/docs/orphaned/Web/JavaScript/Reference/Global_Objects/Array/filter'>filter</a>: 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환한다. `arr.filter(callback(element[, index ?[, array ?]])[, thisArg ?])`

원본 배열을 변경하고 특정 인덱스를 제거하고 싶을 때(새로운 변수에 선언하고 싶을 않을 때)
1. <a href='https://developer.mozilla.org/ko/docs/orphaned/Web/JavaScript/Reference/Global_Objects/Array/splice'>splice</a>: 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경한다. `(array.splice(start, delete ?, item ?))`

원본 배열을 변경하고 싶지않고 특정 범위를 잘라내고 싶을 때
1. <a href='https://developer.mozilla.org/ko/docs/orphaned/Web/JavaScript/Reference/Global_Objects/Array/slice'>slice</a>: 어떤 배열의 `begin`부터 `end`까지(`end`는 미포함)에 대한 얕은 복사(shallow copy)본을 새로운 배열 객체로 반환시킨다. `arr.slice(begin ?, end ?)`

정의만 봐서는 잘 이해가 안되니까 쉽게 예를 들어보자. 다음의 `arr`에서 나는 `arr[2]`의 값을 제거하고, `arr`의 `[1 : 3]`범위만 자르고 싶다.

```javascript
const arr = [0, 2, 1, 6]

// filter
const filterArr = arr.filter((item) => item !== arr[2])
console.log(filterArr)
👉🏽 [0, 2, 6]

// splice
arr.splice(2)
console.log(arr)
👉🏽 [0, 2, 6]

// slice
const sliceArr = arr.slice(1, 3)
console.log(sliceArr)
👉🏽 [2, 1]
```

여담으로 `slice`는 원본 객체를 `얕은 복사(shallowCopy)`까지 가능한데, 정말 `얕은 복사(shallowCopy)` 까지만 되는지 실험해봤다. 실험결과 `depth = 2`인 값을 변경시 원본값도 변경된것으로 보아 얕은복사까지만 가능했다.

```javascript
// depth = 1일 때 slice
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
const copyArr = arr.slice();
arr[2] = -9999

console.log(copyArr)
👉🏽 [1, 2, 3, 4, 5, 6, 7, 8, 9];

// depth = 2일 때 slice
const arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];
const copyArr = arr.slice();
arr[0][2] = -9999

console.log(copyArr)
👉🏽 [[1, 2, -9999], [4, 5, 6], [7, 8, 9]];
```

---
### 📍 일급객체(First Class Object)에 대해서 간략하게 알아보자
프론트엔드 신입 면접에서 `First Class Object`가 무엇인지 설명해주세요. 라는 첫 질문을 듣고 머리가 벙쪘다. 용어를 들어봤으면 관련된 내용을 쥐어짜내기라도 할 텐데 도무지 생각이 나지 않았기 때문이다.

![](https://images.velog.io/images/abcd8637/post/b1f82ec5-8b0b-43dc-85ee-c70e3ceeb1a5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-09-03%2006.48.36.png)

답답함을 이기지 못하고 면접관분께 시원하게 말씀드렸다. 그런 용어를 처음 듣습니다.. 공부를 더 해야겠습니다.. 면접관님은 일급 객체에 대해 자세히 설명을 해주셨다. 그리고 밀려오는 자괴감..

![](https://images.velog.io/images/abcd8637/post/dbfc2358-95f7-45f3-a300-66ca7d904472/More_details_be_omitted.jpeg)

비록 면접에 떨어진 이유가 일급객체를 몰라서 떨어진것은 아니지만 `JS`의 기초적인 내용을 많이 공부해야겠다는 생각이 들었다. 그래서 알아보자 일급객체(`First class object`)란 무엇인가?

`MDN`에서 찾아보니까 다음과 같은 정의가 나온다. `함수`를 변수처럼 다루는 언어는 일급 함수를 가졌다고 표현한다. 이 문장을 보고 바로 `함수 표현식`이 생각났다. 함수표현식과 함수선언식의 차이는 안다고 우쭐댔지만 정작 `일급객체`를 모르는 나.. 

![](https://images.velog.io/images/abcd8637/post/7d52d695-9b60-4ee8-a90d-4b2f3fb18cb2/jjvNX.jpeg)

이외에도 다음과 같은 경우를 만족하면 일급 객체라고 부른다. 코드와 함께 살펴보자.

1. Functions can be assigned to variables: 함수를 변수처럼 선언 할 수 있는가?

```javascript
const sayHi = function(){
    return "Hi";
}

console.log(sayHi());
👉🏽 Hi
```

2. Functions can be passed to arguments to other functions: 함수의 인자로 함수를 넣을 수 있는가?
```javascript
const sayHelloToPerson = (greet, person) => {
    return greet() + " " + person
}

const sayGreet = function(){
    return "Hello,"
}

console.log(sayHelloToPerson(sayGreet, "Ted"));
👉🏽 Hello, Ted
```

3. Functions can be returned from other functions: 함수의 리턴 값으로 함수를 사용 할 수 있는가?
```javascript
const sayHello = function(){
    return function(greet){
        return greet
    }
}

const sayHelloOuter = sayHello();
const sayHelloInner = sayHelloOuter("Hello");
console.log(sayHelloInner)
👉🏽 Hello

// closure를 사용한 방법
const sayYouAndMe = function(yourName){
    return function(myName){
        return yourName + "과 " + myName;
    }
}

const sayYourName = sayYouAndMe("elon");
const sayMyName = sayYourName("Ted");
console.log(sayMyName);
👉🏽 elon과 Ted
```

reference
1. <a href ='https://developer.mozilla.org/ko/docs/Glossary/First-class_Function'>MDN</a>
2. <a href='https://www.youtube.com/watch?v=4UeWzn4jzwM'>First Class Functions in JavaScript - Youtube</a>

---
### 📍 메서드 내부 함수에서 this를 우회하는 방법 4가지
만약, `scope chain`처럼 변수가 없을 때 가장 가까운 스코프의 `Lexical Environment`를 찾고, 거기도 없으면 상위 스코프를 탐색하듯이 `this`도 호출 주체가 없을 때는 자동으로 전역객체를 바인딩하지 않고 호출 당시 주변 환경의 `this`를 상속하고 싶다면 어떤 방법을 사용할까? 현재 컨텍스트에 바인딩된 대상이 없으면 직전 컨텍스트의 `this`를 바라보는 것처럼 말이다. 아쉽게도 `ES5` 까지는 자체적으로 내부함수에 `this`를 상속할 방법은 없지만 우회하는 방법은 있다. 지금부터 메서드의 내부함수에서 메서드의 `this`를 그대로 바라보게 하기 위한 방법 4가지를 알아보자. 그리고 `ES6`에서 `this`를 바인딩하지 않고 상위 스코프의 `this`를 그대로 활용가능한 `화살표 함수(arrow-function)`로도 사용한 코드를 살펴보자. 결과는 모두 동일하다.

1. `this` 변수 저장
2. `call`
3. `bind`
4. `화살표함수`

```javascript
// this 변수 저장
var obj = {
    outer: function () {
        console.log(this);

        var self = this;
        var innerFunc = function () {
            console.log(self);
        };
        innerFunc();
    },
};
obj.outer();

// call
var obj = {
	outer: function(){
		console.log(this);
		var innerFunc = function(){
			console.log(this);
		}
		innerFunc.call(this);
	}
}
obj.outer();

// bind
var obj = {
	outer: function(){
		console.log(this);
		var innerFunc = function(){
			console.log(this);
		}.bind(this);
		innerFunc();
	}
}
obj.outer();

// arrow Function
var obj = {
    outer: function () {
        console.log(this);
        var innerFunc = () => {
            console.log(this);
        };
        innerFunc();
    },
};
obj.outer();


👉🏽 { outer: [Function: outer] }
👉🏽 { outer: [Function: outer] }
```

reference
1. <a href='https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=206513031'>코어 자바스크립트 - 정재남</a>

---
### 📍 2차원 배열 선언하기
`Array.from` 메서드를 사용하여 2차원 배열을 선언 할 수 있다. 예를 들어 3행 4열을 0으로 초기화한 2차원 배열을 만들고 싶다면 다음과 같이 작성한다.

```javascript
let row = 3;
let column = 4;
let arr = Array.from(Array(row), ()=> Array(column).fill(0));

console.log(arr)
👉🏽 [ [ 0, 0, 0, 0 ], [ 0, 0, 0, 0 ], [ 0, 0, 0, 0 ] ]
```

reference
1. <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from'>MDN</a>

---
### 📍 DOM 속성에 이벤트 핸들러 연결하기
이벤트 핸들러를 DOM요소에 연결 할 때 보통 2가지 방법을 사용하는데, 첫번째로 `on`속성과, 두번째는 `addEventListener`이다. 전자의 경우 빠르지만 지저분한 방법이라고 말한다. 예를 들어 `ondblclick`, `onmouseover`, `onblur`, `onfocus` 속성도 있다.

```javascript
const button = document.querySelector("button");
button.onclick = () => {
    console.log("Click managed using onclick property");
}
```

![](https://images.velog.io/images/abcd8637/post/5b42d540-0dce-47f7-adb6-51e47cbbf645/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-21%2021.46.50.png)

이런 해결책은 잘 동작하더라도 일반적으로 나쁜 관행으로 관주되는데, 그 이유는 속성을 사용하면 한번에 하나의 핸들러만 연결할 수 있기 때문이다. 따라서 코드가 `onclick` 핸드러를 덮어 쓰면 원래 핸들러는 영원히 손실된다. 그래서 더 나은 접근 방식인 `addEventListener` 메서드를 사용한다.

`addEventListener`의 첫 번째 매개변수는 이벤트 타입이고, 두번째 매개변수는 콜백이며 이벤트가 트리거될 때 호출된다. 다음 코드처럼 모든 핸들러를 연결 할 수 있다.

```javascript
const button = document.querySelector("button")
const firstHandler = () => {
    console.log("First Handler");
}
const secondHandler = () => {
    console.log("Second Handler");
}

button.addEventListener("click", firstHandler)
button.addEventListener("click", secondHandler)
```

![](https://images.velog.io/images/abcd8637/post/0d5b2de5-bd9b-4654-a2da-423bea62d2f1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-21%2021.46.22.png)

나아가 `DOM`에 요소가 더 이상 존재하지 않으면 메모리 누수를 방지하고자 이벤트 리스너를 삭제해야하는데, 이때 `removeEventListener` 메서드를 사용한다. 다음 코드에서 가장 중요한 점은 이벤트 핸들러를 제거하려면 `removeEventListener`메서드에 매개변수로 전달할 수 있도록 이에 대한 참조를 유지해야 한다는 것이다.

```javascript
const button = document.querySelector("button")
const firstHandler = () => {
    console.log("First Handler");
}
const secondHandler = () => {
    console.log("Second Handler");
}

button.addEventListener("click", firstHandler)
button.addEventListener("click", secondHandler)

window.setTimeout(() => {
    button.removeEventListener('click', firstHandler)
    button.removeEventListener('click', secondHandler)
    console.log("Removed Event Handlers");
 }, 1000)
```

![](https://images.velog.io/images/abcd8637/post/c4230ec0-f042-46f5-9fe6-2c9eb71707c8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-21%2021.52.16.png)

Reference
1. <a href='https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=260034588'>프레임워크 없는 프론트엔드 개발 - 프란세스코 스트라츨로</a>
2. https://developer.mozilla.org/ko/docs/Web/API/EventTarget/removeEventListener

---
### 📍 RegExp(정규표현식)의 개념과 응용 예제 살펴보기
`RegExp`는 정규표현식이라고 부르는데 문자열을 대상으로 패턴 매칭 기능을 제공한다. 패턴 매칭 기능이랑 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다. 여러 방면에서도 사용되지만 나는 알고리즘 문제를 풀거나 `util` 관련 함수를 만들 때 주로 사용한다. 정규표현식의 장점은 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있다. 하지만, 여러가지 기호를 혼합하여 사용하기 때문에 처음 접한다면 가독성이 매우 좋지 못한 단점이 있다. 정규표현식에는 다양한 패턴이 있지만 그 중 대표적으로 쓰이는 것들만 알아보자.

본론으로 들어가기 전에 정규 표현식의 검색 방식을 설정하는 플래그가 있는데, 총 6개의 플래그가 있다. 플래그는 여러개 넣을 수 있으며, 플래그를 사용하지 않은 경우에는 대소문자를 구분하여 패턴을 검색한다. 또한 검색 매칭 대상이 여러개여도 첫 번째 매칭 대상만 검색하고 종료한다.(여러개의 매칭 결과를 반환하려면 `g`플래그를 사용하면 된다.)

```javascript
1. `g`(global): 패턴과 일치하는 모든 값들을 전역 검색한다. (반성문을 영어로 해석한 것이 아님 🙃)
2. `i`(ignoreCase): 대소문자를 구분하지 않고 패턴을 검색한다.
3. `m`(multiLine): 문자열의 행이 바뀌더라도 패턴 검색을 지속한다.
4. `s`(source): 개행 문자인 `\n`도 포함하도록 활성화 한다.
5. `u`(unicode): 유니코드 전체를 지원한다.
6. `y`(sticky): 문자 내 특정 위치에서 검색을 진행하는 `sticky` 모드를 활성화 시킨다.
```

하나만 더 알아보자. 바로 패턴인데, 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용된다면, 정규표현식의 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용한다. 패턴은 문자열의 따옴표 대신 `/`로 열고 닫는다. 즉, `"Hello"`가 아니라 `/Hello/`로 처럼 사용하는 것이다. 만약, 따옴표를 포함한다면 따옴표까지도 패턴에 포함되니 이 점에 주의하자. 패턴의 종류는 마찬가지로 여러개 있지만 주로 사용하는 패턴은 다음과 같다.

1. `[ ]` : 괄호안에 있는 값은 `or` 을 나타냄
2. `\d`: [ 0-9 ]를 의미함
3. `\D`: 숫자가 아닌 문자를 의미함
4. `\w`: 알파벳, 숫자, 언더스코어를 의미함( `[ A-Za-z0-9 ]`)
5. `\W`: 알파벳, 숫자, 언더스코어가 아닌 문자를 의미함
6. `/s`: 여러가지 공백 문자(스페이스, 택 등)를 의미한다. `[/t/r/n/v/f]` 와 같은 의미다.
7. `*`: 이전 항목을 0번 이상 반복함
8. `+`: 이전 항목을 1회 이상 반복함
9. `^`: `[ ]` 내부에 있는 `^` 는 `not` 을 의미하고, `[ ]` 외부에 있는 `^` 는 문자열의 시작을 의미한다.
10. `$`: 문자열의 마지막을 의미한다.
11. `?`: 앞선 패턴이 최대 한 번 이상(0번 포함) 반복되는지를 의미한다. 즉, 앞선 패턴 (`s`)이 있거나 없어도 매치된다.
12. `{m,n}`: 반복 검색 패턴으로 최소 `m`번, 최대 `n`번 반복되는 문자열을 의미한다. 콤마뒤에 공백이 오지 않게 주의하자.

자, 이제 플래그와 패턴에 대해서 알아봤으니 본격적으로 `RegExp` 메서드에 대해 알아보자. <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp'>MDN</a>에 적혀있듯이 메서드도 여러개 있지만, 여기서는 `exec`, `match`, `test`정도만 알아보자. 

1. `exec`: 매칭 결과를 배열로 반환한다. 매칭 결과가 없는 경우 `null`로 반환한다. 특이사항으로는 `g` 플래그를 사용하더라도 첫 번째 매칭 결과만 반환한다.

```javascript
let target = "is";
let str = "what is your name? my name is ywtechit";

console.log(/is/g.exec(target));
👉🏽 [ 'is', index: 0, input: 'is', groups: undefined ]
```

2. `match`: 주어진 문자열에 대해 일치하는 결과를 반환한다. `exec` 메서드와는 다르게 `g` 플래그를 지정하면 모든 매칭 결과를 배열로 반환한다.

```javascript
// ex1: 문자 'p'의 개수를 return 하시오
let lyrics = "i have a pen pineapple apple pen";
let reg = lyrics.match(/p/g) // [ 'p', 'p', 'p', 'p', 'p', 'p', 'p' ]
console.log(reg.length)
👉🏽 7  // 배열의 길이가 곧 `p`의 개수와 동일하다.

let target = "what is your name? my name is ywtechit";

// ex2: is 값을 한번만 찾기
console.log(target.match(/is/));
👉🏽 [ 'is', index: 5, input: 'what is your name? my name is ywtechit', groups: undefined ]

// ex3: is 값을 모두 찾기
console.log(target.match(/is/g));
👉🏽 [ 'is', 'is' ]

// ex4: A가 2번이상 반복하는 문자열을 반환하기
const target = "A AA B BB Aa Bb AAA";
const regExp = /A{2,}/g;
const result = target.match(regExp)
console.log('result :>> ', result);  // [ "AA", "AAA" ]

// ex5: URL 제일 마지막 값만 추출하는 정규표현식
const url = "https://swtrack.elice.io/courses/16306/lecturerooms/16157";
const reg = url.match(/\/([0-9]+)$/);

console.log(reg);
👉🏽
[
  0: '/16157',
  1: '16157',
  2: index: 51,
  3: input: 'https://swtrack.elice.io/courses/16306/lecturerooms/16157',
  4: groups: undefined
]

// ex6: URL에 포함된 특정 문자 분리하기
let reg = s.match(/\/(lecturerooms)\/([0-9]+$)/);
console.log(reg);
👉🏽
[
	0: "/lecturerooms/16157"
	1: "lecturerooms"
	2: "16157"
	groups: undefined
	index: 38
	input: "https://swtrack.elice.io/courses/16306/lecturerooms/16157"
	length: 3
]

// ex7: 문자의 내용과 상관없이 3자리 문자열과 매치하기
let target = "Is this your mac? Is this your phone?";
console.log(target.match(/.../g));
👉🏽
[
  'Is ', 'thi', 's y',
  'our', ' ma', 'c? ',
  'Is ', 'thi', 's y',
  'our', ' ph', 'one'
]
```

3. `test`: 매칭결과를 불리언(boolean)값으로 반환한다. 지금까지 이 메서드를 제일 많이 사용했다. 여담으로 올바른 이메일 형식인지 검사하는 공식 패턴(FC 5322 Official Standard)은 <a href='https://emailregex.com/'>emailregex.com</a>에서도 확인 할 수 있다.

```javascript
// ex1: 해당 문자열이 있는지 검사하기
let target = "what is your name? my name is ywtechit";

console.log(/is/.test(target));  // true

console.log(/typescript/.test(target));  // false

// ex2: new 연산자를 사용한 RegExp와 new 연산자를 사용하지 않은 RegExp
const str = 'table football';
const regex = new RegExp('foo*');
const sameRegex = /foo*/;
const globalRegex = new RegExp('foo*', 'g');

console.log(regex.test(str));  // true

console.log(globalRegex.lastIndex);  // 0

console.log(globalRegex.test(str));  // true

console.log(globalRegex.lastIndex);  // 9

console.log(globalRegex.test(str));  // false

// ex3: 올바른 전화번호 형식인지 판별하는 패턴
const tel = "010-1234-1234"
const regExp = /^\d{3}-\d{4}-\d{4}$/;
const result = regExp.test(tel);
console.log('result :>> ', result);  // true

// ex4: http://, https://로 시작하는 도메인인지 판별하는 패턴
const domain = "https://www.naver.com";
const regExp = /^(http|https):\/\//;
const regExp2 = /^https?:\/\//;
const result = regExp.test(domain);
console.log('result :>> ', result);  // true

// ex5: html로 끝나는 파일명인지 판별하는 패턴
const string = "index.html";
const regExp = /html$/
const result = regExp.test(string)
console.log('result :>> ', result);  // true

// ex6: 소수점을 최대 2번까지만 사용하는 입력인가?
const isUptoTwoDecimalPoint = (input: string): boolean => {
  const isIncludeDot = /[.]/g;

  if (isIncludeDot.test(input)) {
    const isTwoDecimalPoint = /^\d+[.]\d{1,2}$/;
    if (isTwoDecimalPoint.test(input)) return true;
    return false;
  }
  return true;
};

// ex7: 하나 이상의 공백으로 시작하는가?
const string = " Hi!!";
const regExp = /^[\s]+/
const result = regExp.test(string)
console.log('result :>> ', result);  // true

// ex8: 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인가?
const string = "abcd8637";
const regExp = /^[\w]{4,10}$/
const result = regExp.test(string)
console.log('result :>> ', result);  // true

// ex9: 올바른 이메일 형식인가?
const string = "ywtechit@gmail.com";
const regExp = /^[\w]([-_\.]?[\w])*@[\w]([-_\.]?[\w])*\.[a-zA-Z]{2,3}$/
const result = regExp.test(string)
console.log('result :>> ', result);  // true ​​​​​at

// General Email Regex (RFC 5322 Official Standard)
const regExp = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;

// HTML5 input 태그의 type=”email”에 사용되는 패턴(from W3C: Ref 5.)
<input type="email" placeholder="Enter your email" />
const regExp = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/

// ex10: 특수문자 판별하기
let s = "(A(BC)D)EF(G(H)(IJ)K)LM(N)";
let answer = "";

console.log(solution(s));

function solution(s) {
  for (let x of s) {
    if (/[A-Z]/.test(x)) answer+=x;
  }
  return answer;
}

👉🏽 ABCDEFGHIJKLMN
```

4. `replace`: 주어진 문자열내에 일치를 새로운 문자열로 대치하는 메소드

```javascript
// ex1: 공백 제거하기
const str =
  "나랏말싸미 듕귁에 달아문자와로 서르 사맛디 아니할쎄 이런 전차로 어린 백셩이 니르고져 홇베이셔도 마참네 제 뜨들 시러펴디 몯핧 노미하니아 내 이랄 윙하야 어엿비너겨 새로 스믈 여듫 짜랄 맹가노니사람마다 해여 수비니겨 날로 쑤메 뻔한킈 하고져 할따라미니라";
const result = str.replace(/\s/g, "")

console.log('result :>> ', result);
👉🏽 나랏말싸미듕귁에달아문자와로서르사맛디아니할쎄이런전차로어린백셩이니르고져홇베이셔도마참네제뜨들시러펴디몯핧노미하니아내이랄윙하야어엿비너겨새로스믈여듫짜랄맹가노니사람마다해여수비니겨날로쑤메뻔한킈하고져할따라미니라

// ex2: 특수문자 제거하기
let s = "found7, time: study; Yduts; emit, 7Dnuof";
let notIncludeSpecialCharacter = s.replace(/[^A-z]/g, '');
👉🏽 foundtimestudyYdutsemitDnuof

// ex3: 특수문자 중에서 공백은 살리고 싶을 때
let notIncludeSpecialCharacter = s.replace(/[^A-z | " "]/g, '');
👉🏽 found time study yduts emit dnuof
```

지금까지 정규표현식의 개념과 응용 예제를 간략하게 살펴보았다. 처음 정규표현식을 접했을 땐 도통 이해가 되지 않은 문법이었는데, 몇번 사용해버릇하니까 반복문, 조건문보다 이용하기가 쉽고 편했다. 조금씩 정규표현식을 사용하며 경험을 쌓고 나중에 현업에 도입하게되면 예제를 까먹지 않도록 기록해둬야 겠다.

Reference
1. https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp
2. https://ko.javascript.info/regexp-introduction
3. https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=251552545
4. https://emailregex.com/
5. https://html.spec.whatwg.org/multipage/input.html#input.email.attrs.value.multiple

---
### 📍 terminal에서 npm private 패키지를 다운받을 수 없을 때
`React`에서 `npm i`를 통해 필요한 패키지를 다운받으려는데 `Not found` 에러가 뜨면서 정상적으로 찾지 못했다. 그래서 <a href='https://www.npmjs.com/'>npm.js</a>에 들어가보니 하단의 사진처럼 패키지가 정상적으로 등록은 되어있었다.

![](https://velog.velcdn.com/images/abcd8637/post/5008b76f-dc51-4a41-84b0-089373522388/image.png)

결국, `terminal`에서 `npm login`을 하지 않아서 패키지를 다운받지 못한 것이었는데, 생각해보니 `private`한 패키지를 현재 요청하는 사용자의 권한이 있는지도 확인하지 않은 채 다운로드 받을 수 있다면 패키지를 `private`로 설정한것이 무슨 소용이 있을까라는 생각이 들었다. `npm login` 커맨드 입력 후 자신의 <a href='https://www.npmjs.com/'>npm.js</a>계정을 입력하면 `Logged in as <id>`커맨드가 뜨는데 정상적으로 `terminal`에 `npm` 계정이 등록된 것을 확인 할 수 있다.

![](https://velog.velcdn.com/images/abcd8637/post/44b75373-9873-44c3-b478-92efe0f09883/image.png)

`terminal`에 자신의 계정이 성공적으로 등록됐는지 확인하려면 `npm whoami`명령어로 확인하자.

![](https://velog.velcdn.com/images/abcd8637/post/9b2ee914-0a26-4a97-b8d5-e01ea5f42cac/image.png)

Referenced
1. https://docs.w3cub.com/npm/getting-started/installing-node.html
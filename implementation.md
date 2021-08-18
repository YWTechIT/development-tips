# typescript_tips

## 📍 interface, type 사용하기

```ts
interface MyInfo {
    name: string;
    age: number;
}

const myInfo: MyInfo = {
    name: 'AYW',
    age: 27,
}

type YourInfo = {
    name: string;
    age: number;
}

const yourInfo: YourInfo = {
    name: 'JBY',
    age: 25,
}
```

---
## 📍 배열타입 선언하기
1. 첫번째 방법: 제네릭은 type에 변수를 제공하는 방법이며 제네릭이 없으면 어떤 것이든 포함 할 수 있다.
2. 두번째 방법: 배열 요소들을 나타내는 타입 뒤에 `[]`를 사용한다.

```ts
type StringArray = Array<string>;
type StringArray2 = string[];

const myFamily: StringArray = ['father', 'mother', 'brother']
const myFamily2: StringArray = ['father', 'mother', 'brother']

type NumberArray = Array<number>;
type NumberArray2 = number[];

const myFamilyAges:NumberArray = [50, 48, 27, 25]
const myFamilyAges2:NumberArray2 = [50, 48, 27, 25]

type ObjectWithAddressArray = Array<{address: string}>;
type ObjectWithAddressArray2 = {address: string}[];

const myInfo: ObjectWithAddressArray = [{address: 'HWASEONG'}]
const yourInfo: ObjectWithAddressArray2 = [{address: 'INCHEON'}]

console.log(myFamily, myFamily2)
👉🏽 ['father', 'mother', 'brother'], ['father', 'mother', 'brother']

console.log(myFamilyAges, myFamilyAges2)
👉🏽 [50, 48, 27, 25], [50, 48, 27, 25]

console.log(myInfo, yourInfo)
👉🏽 [{address: 'HWASEONG'}], [{address: 'HWASEONG'}]
```

---
## 📍 interface 사용하지 않고 써보기 사용하고 써보기

```ts
// do not use Interface
const getMathScore = (scoreObj: {math: number}): number => {
    return scoreObj.math
}

const myScore = {korean: 85, math: 95, english: 87}
const myMathScore = getMathScore(myScore);

console.log(myMathScore)
👉🏽 95

// do Interface
interface PhysicalValue {
    physical: number;
}

const getPhysicalScore = (scoreObj: PhysicalValue) => {
    return scoreObj.physical
}

const yourScore = {music: 87, art: 69, physical: 92}
const yourPhysicalScore = getPhysicalScore(yourScore);

console.log(yourPhysicalScore)
👉🏽 92
```

---
## 📍 TodoListItem Component 생성 후 props로 constant 보내기

1. `TodoListItem` 컴포넌트를 생성한다.
   
```ts
import React from 'react';

const TodoListItem = () => {
    return(
        <>
        <label>
            <input />
        </label>
        </>
    )
}
```

---
## 📍 타입 주석과 타입 추론
`TS`에서 `1번` 코드처럼 콜론(`:`)과 타입을 붙이는데 이를 `타입 주석(type-annotation)`이라고 한다. 반면 `2번` 코드처럼 타입 부분이 생략되면 대입 연산자 오른쪽 값을 분석해 왼쪽 변수의 타입을 결정하는데 이를 `타입 추론(type interface)`라고 한다. 이 기능은 `JS`와 호환성을 보장하는데 큰 역할을 한다. 타입 추론 덕분에 `.js` 파일을 `.ts`로 바꿔도 `TS` 환경에서도 바로 동작한다.

```ts
let number: number = 1;    // 타입 주석(type annotation)
let testInterface = 3;    // 타입 추론(type interface)

testInterface = '123';    // type 불일치 오류
```

---
## 📍 vscode에서 Cannot use JSX unless the '--jsx' flag is provided 뜰 때
처음 `TS`를 설치하고 작업을 하던 도중 `yarn start`를 치니까 갑자기 `tsx`파일에 빨간 줄이 뜨면서 제목과 같은 문구가 나왔다. 결론적으로 `TS` 버전이 맞지 않아 생기는 오류인데 제일 간단한 방법은 `VScode` 하단에 최신 버전으로 적용하는 방법이다.

1. 우측에서 네번째 버전을 클릭한다.

![](https://images.velog.io/images/abcd8637/post/eb40eaff-5764-41ee-8c7f-96389c0fcc7f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2010.55.21.png)

2. 위에서 두번째 `Select TypeScript version...`을 클릭하자. 

![](https://images.velog.io/images/abcd8637/post/9dc25671-6e48-45e3-8e5e-e231f166f11b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2010.51.46.png)

3. 최신 버전으로 눌러주면 된다.

![](https://images.velog.io/images/abcd8637/post/fe2916a3-9ef1-4926-87c4-a134bb112a8f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2010.51.54.png)

4. 적용 성공

![](https://images.velog.io/images/abcd8637/post/f3a7a334-b19b-415a-9eef-6951f9bf8aa2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2010.52.05.png)

그러나 버전이 뜨지 않아 적용이 안되면 다음과 같이 새로 설치해주자.

1. `terminal`에 `npm install -g typescript`를 입력한다.

2. `npm list -g typescript`를 입력해 어떤 경로에 설치되었는지 확인한다.

![](https://images.velog.io/images/abcd8637/post/577eefc4-ac11-4994-9791-2e696df548fa/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2010.59.12.png)

3. `vscode`에서 `settings.json`파일에 들어간다(MAC은 `command + shift + p`)

![](https://images.velog.io/images/abcd8637/post/1d991a45-6096-4632-ab13-1a18ffb5ce1e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2011.02.35.png)

4. `"typescript.tsdk": "2번에서 확인한 경로/node_modules/typescript/lib"`을 추가한다.

![](https://images.velog.io/images/abcd8637/post/04689df6-f89d-4c11-8302-9c8eada3edc8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2011.03.13.png)

5. 맨 위의 방법을 다시해본다.(버전 클릭 -> 최신 버전 입력)

6. Happy Hacking ✨

> reference: <a href='https://chacha73.tistory.com/44'>Tistory</a>, <a href='https://stackoverflow.com/questions/50432556/cannot-use-jsx-unless-the-jsx-flag-is-provided'>stackoverflow</a>

---
## 📍 form 태그를 이용하여 state 관리하기
`TS`를 이용하여 `Todo-list`를 만들다가 이전까지는 `button`의 `onClick`을 이용하여 `input` 값을 보냈는데 이번에는 `form`태그를 사용하여 값을 보내봤다. `form`태그를 사용하면 `input`창의 값을 보낼 때 `onKeyPress(enter)` 함수를 따로 만들지 않아도 돼서 유용했다. 웬만하면 `useState`, `useRef`가 어떤 타입인지도 명시하자.

또, 함수내에서 `event`를 받아올때 `eventType`이 무엇인지 알고 싶으면 `event`를 처리하는 태그에 마우스를 올리면 타입을 어떻게 적용해야하는지 다음 사진처럼 팝업창이 뜬다.

![](https://images.velog.io/images/abcd8637/post/72030866-30f0-412e-a5db-49f70fb94335/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-20%2022.19.48.png)

팁으로 `15번 라인`처럼 함수내에서 함수를 선언 해야하는 경우 가독성을 높이기 위해 `utility` 폴더를 따로 만들어서 저장하고 `import` 하는 방법을 사용했다.

![](https://images.velog.io/images/abcd8637/post/caa68edd-355b-47fa-a2aa-a7258d1825be/Jul-20-2021%2022-28-30.gif)

```javascript
interface Todo {
  id: string;
  text: string;
  done: boolean;
}

const App = () => {
    const [todos, setTodos] = useState<Todo[]>(TODO_CONSTANT);
    const nextId = useRef<number>(todos.length + 1);
    const [title, setTitle] = useState<string>("");

    const handleOnSubmit = (e: React.FormEvent<HTMLFormElement>) => {
        e.preventDefault();
        const submitTodo: Todo = getSubmitTodo(nextCurrentId.current, title);
        setTodos(todos.concat(submitTodo));
        setTitle("");
        nextId.current += 1;
  };

    const handleEvent = (e: React.ChangeEvent<HTMLInputElement>) => {
        setTitle(e.target.value);
    };

    return(
    <>
        <form onSubmit={handleOnSubmit}>
            <li>
                <input
                    placeholder="값을 입력하세요."
                    onChange={handleEvent}
                    value={title}
                />
                <button>추가!</button>
            </li>
        </form>
    </>
    )
}

// utility/getSubmitTodo/index.tsx
const getSubmitTodo = (nextId: number, title: string) => {
  const submitTodo = {
    id: String(nextId),
    text: title,
    done: false,
  };
  return submitTodo;
};

export default getSubmitTodo;
```

---
## 📍 customHooks로 getApi 코드 관리하기
`fetch` 혹은 `axios`를 사용하여 `api`를 받아올 때 기존에는 `app.tsx` 파일내에서 `useEffect`를 사용하여 `api`를 받아오는 코드를 모두 작성했다. 그러나, `app.tsx`에 `api`와 관련된 코드를 모두 작성하면 다른 로직코드를 보는데 신경쓰여서 다른곳으로 옮겨야겠다는 생각이 떠올랐고 `hooks` 폴더를 생성하여 `customHooks`로 따로 관리하는 방법이 `app.tsx`의 가독성을 증가시킨다고 생각했다.

`data`, `loading`, `error` 값을 `export` 할 때 타입추론이 명확하게 되지 않는경우가 있어 `as`문을 사용해서 어떤 타입인지 명시했다. `as`문 대신 `interface`를 `usePosts` 리턴값에 붙여줘도 되는데 이때는 `{}`로 `return` 하고 `app.tsx`에서 불러올 때도 `{}`로 불러오는것을 잊지말자.

```typescript
// hooks/usePosts/index.tsx
// use alias(as)
import axios, { AxiosResponse } from "axios";
import { useEffect, useState } from "react";
import { Data } from "../../types";

const usePosts = () => {
  const [data, setData] = useState<Data[]>([]);
  const [loading, setLoading] = useState<boolean>(false);
  const [error, setError] = useState<Error>();

  useEffect(() => {
    setLoading(true);
    const getPosts = async () => {
      try {
        const response: AxiosResponse<any> = await axios.get(
          "https://jsonplaceholder.typicode.com/posts/"
        );
        setData(response.data);
        setLoading(false);
      } catch (e) {
        setError(e);
        setLoading(false);
      }
    };
    getPosts();
  }, []);

  return [data, loading, error] as [Data[], boolean, Error];
};

export default usePosts;
```

```typescript
// hooks/usePosts/index.tsx
// use interface
import axios, { AxiosResponse } from "axios";
import { useEffect, useState } from "react";
import { Data } from "../../types";

interface Response {
  data: Data[];
  loading: boolean;
  error?: Error;
}

const usePosts = (): Response => {
  const [data, setData] = useState<Data[]>([]);
  const [loading, setLoading] = useState<boolean>(false);
  const [error, setError] = useState<Error>();

  useEffect(() => {
    setLoading(true);
    const getPosts = async () => {
      try {
        const response: AxiosResponse<any> = await axios.get(
          "https://jsonplaceholder.typicode.com/posts/"
        );
        setData(response.data);
        setLoading(false);
      } catch (e) {
        setError(e);
        setLoading(false);
      }
    };
    getPosts();
  }, []);

  return {data, loading, error};
};

export default usePosts;

```

```typescript
// app.tsx
import Container from "./components/container";
import Card from "./components/molecules/card";
import usePosts from "./hooks/useposts";

const App = () => {
  const [data, loading, error] = usePosts();

  if (loading) {
    return <div>loading...</div>;
  }

  if (error){
    return <div>Error...</div>
  }

  return (
    <Container>
      {data.map((item) => (
        <Card key={item.id} item={item}></Card>
      ))}
    </Container>
  );
};

export default App;
```




## 📍 불필요한 prop drilling 줄이기
`React`에서 페이지 내부에 2~3개의 컴포넌트를 선언하며 `props`로 전달하는 경우가 있을 것이다. 이번 글은 한 페이지 내에 컴포넌트 3개(`A` -> `B` -> `C`)를 선언하여 단순히 `props`로 전달만하는 `B`컴포넌트의 `prop drilling`을 어떻게 해결했는지 알아보자.

우선, `React`를 다뤄 본 개발자라면 `prop drilling`에 대해 한 번쯤은 들어봤을 것이다. `prop drilling`이란, `props`를 이용해 자식 컴포넌트로 데이터를 내려줄 때 모든 레벨에서 같은 데이터가 전송되는 상황을 말한다.

![](https://velog.velcdn.com/images/abcd8637/post/fa003998-d2fb-40db-b998-cc496bef0acf/image.png)

규모가 작을 때 `prop drilling`은 빠르고 쉽게 데이터를 전송할 수 있고, 구현 방법이 쉽고, `props`로 전달된 데이터의 상태 변경 시 새로운 변경사항을 쉽게 업데이트 할 수 있는 장점이 있다. 그러나 자식 컴포넌트가 부모 컴포넌트의 데이터를 전달해주는 역할로만 사용되고 그 때문에 코드가 불필요하게 길어지며 필요하지 않은 데이터까지 전달해줄 수 있기 때문에 프로젝트의 규모가 커질수록 권장하지 않는 방법이다. 

이럴 경우 `contextAPI` 혹은 전역상태관리 라이브러리의 도움을 받을 수 있지만, 이번엔 그들의 도움을 받지 않고 코드를 어떻게 줄였는지를 예시를 들어 살펴보자.

우선, `API`를 이용해 `User`가 가입한 계정을 찾는 `Accounts` 데이터를 가져온다고 가정해보자. 

필자는 처음 `AccountsFound`란 컴포넌트에서 `User`의 `Accounts`를 가져온다. 그리고 같은 데이터의 `Accounts`가 담겨있는 데이터를 `Accounts`란 컴포넌트로 넘기고 이들을 하나씩 보여주기 위해 `map`함수를 사용해 `Account`라는 컴포넌트로 전달해주었다. 아래 코드블록을 보자.

```typescript
// before
interface AccountType {
  name: string
  email: string 
  createdAt: Date
}

function AccountsFound({ accounts }: { accounts: AccountType[] }) {
  return <Accounts accounts={accounts} />
}

function Accounts({ accounts }: { accounts: AccountType[] }) {
  return (
    <>
      <div>고객님의 계정 정보입니다.</div>
      {accounts.map((item, idx) => (
        <Account key={idx} info={item} />
      ))}
    </>
  )
}

function Account({ account }: { account: AccountType }) {
  const { name, email, createdAt } = extractAccountData(account)

  return (
    <>
      <div>{name}</div>
      <div>{email}</div>
      <div>{createdAt}</div>
    </>
  )
}
```

이 페이지에서 작성된 코드 중 비효율적으로 작성된 부분을 찾을 수 있겠는가?

1. 15라인의 `<div>`는 `map`을 통해 `accounts`의 개수만큼 새롭게 렌더링 된다. 같은 데이터를 불필요하게 매번 새롭게 렌더링 된다.
2. 부모(`<Accounts />`) 컴포넌트는 자식(`<Account />`) 컴포넌트에게 단순히 데이터만 전달해주는 역할만 하고 있다. (이 부분이 `propDrilling`) 따라서, `<Accounts />` 컴포넌트를 없앤다. 즉, 컴포넌트의 렌더링 순서를 `AccountsFound -> Accounts -> Account`에서 `AccountsFound -> Account`로 줄여준다.

```typescript
// after
interface AccountType {
  name: string
  email: string 
  createdAt: Date
}

function AccountsFound({ accounts }: { accounts: AccountType[] }) {
  return (
    <>
      <div>고객님의 계정 정보입니다.</div>
      {accounts.map((account, idx) => (
        <Account key={idx} account={account} />
      ))}
    </>
  )
}

function Account({ account }: { account: AccountType }) {
  const { name, email, createdAt } = extractAccountData(account)

  return (
    <>
      <div>{name}</div>
      <div>{email}</div>
      <div>{createdAt}</div>
    </>
  )
}
```

이렇게 해서 `prop drilling`을 전역상태관리로 해결하는 것이 아니라 불필요한 컴포넌트를 제거하면서 코드를 간단히 줄이는 예시를 살펴봤다. 단순히 데이터만 전달해주는 컴포넌트가 많아졌다고 생각하면 `prop drilling`을 하는 것은 아닌지 한 번쯤 자신을 되돌아볼 수 있는 시간을 잠깐이나 가지며 이 글을 마친다. 

Reference
1. https://www.geeksforgeeks.org/what-is-prop-drilling-and-how-to-avoid-it/

---
## 📍 Route에서 props 전달이 안될 때

React Router를 사용할 때 `props`가 정상적으로 전달되지 않으면 어떤 `component`에 `props`를 주고있는지 확인해보자.
나는 1번과 같이 코드를 간결하게 작성하려고 한 줄로 작성했다가 왜 안되나 했더니 `Route`컴포넌트에 `props`를 전달해주는 코드기 때문이다.

```javascript
import Loading from "../pages/Loading";
import React, { useState } from "react";
import { BrowserRouter as Router, Route } from "react-router-dom";


const App = () => {
const [score, setScore] = useState(0);    // declare useState

return(

// (X)
<Route path="/loading" component={Loading} score={score} />   

// (O)
<Route path="/loading">    
    <Loading score={score}></Loading>  
</Route>)}
```

1.  1번 방법으로 작성했을 때  
    
    ![](https://images.velog.io/images/abcd8637/post/1e31a5e8-74d4-42a4-89cd-df35ae506fcb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-28%2016.50.29.png)
    
2.  2번 방법으로 작성했을 때  
    
    ![](https://images.velog.io/images/abcd8637/post/9d3704b9-0045-42a8-8af3-ba1d305bb6af/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-28%2016.47.45.png)
    
---
## 📍 props로 전달한 파라미터가 정상적으로 함수에 적용되지 않을 때

`props`로 전달받은 값들을 새로운 함수를 거쳐서 나올 때 의도한 대로 동작하지 않으면 다음을 확인해보자.

1.  함수 안에 `props`를 바로 적용하진 않았나? - (X)
2.  컴포넌트 안에 함수를 선언하고 `props`로 받은 값들을 파라미터로 넣었는가? - (O)

## 💡 배운 점: 테스트코드의 중요성

이와 같은 문제가 생기지 않기 위해 기능별로 잘라 단계적으로 실행해보면 실패를 줄일 수 있다.

1.  해당 함수만 따로 `console.log`에 옮겨서 내부 기능을 먼저 테스트해본다.
2.  함수 내부적으로 문제가 없다면 `props`에서 받은 변수를 함수에 넣어본다.
3.  어디에서 잘못됐는지 확인한다.

```javascript
// 공통 실행코드
const Loading = ({ shortPower, longPower }) => {
  const eFunction = getCode(shortPower, longPower);
}

// 1. (X)
const getCode = ({ shortScore, longScore }) => {
    let cCode;
    if (shortScore <= 30 && longScore <= 50) {
      cCode = "engineer";
    } else if (shortScore <= 60 && longScore <= 50) {
      cCode = "information";
    } else if (60 < shortScore && longScore < 50) {
      cCode = "infantry";
    } else if (shortScore < 30 && 50 < longScore) {
      cCode = "communication";
    } else if (shortScore < 60 && 50 < longScore) {
      cCode = "armor";
    } else {
        cCode= 'artillery'
    }
    return cCode;
  };
};

// 2. (O)
const getCode = (short, long) => {
  let cCode;
  if (short <= 30 && long <= 50) {
    cCode = "engineer";
  } else if (short <= 60 && long <= 50) {
    cCode = "information";
  } else if (short > 60 && long <= 50) {
    cCode = "infantry";
  } else if (short <= 30 && 50 <= long) {
    cCode = "communication";
  } else if (short <= 60 && 50 <= long) {
    cCode = "armor";
  } else {
    cCode = "artillery";
  }
  return cCode;
};

```

---
## 📍 모바일화면이 벗어날 때

## ⚡️ 구현 팁

웹페이지에서는 내가 설정한 화면 그대로 잘 나오는데 모바일화면에서는 화면이 깨지거나 벗어날 때

1.  index page에서 `viewport` 코드가 잘 있는가?

---

## 💡 배운 점: 코드를 지울 때는 왜 지우는지 생각하자.

`React Hemlet`을 사용하기위해 `public`안에있는 `index.html`내용을 모두 지우고 `App.js`에 새로 작성했지만, 결과적으로 `<meta name="viewport" content="width=device-width, initial-scale=1" />` 해당 코드가 없어 모바일화면은 정상적으로 출력되지 않았다. 만약 지금의 상황과 비슷하다면 `index,html`페이지를 한번 살펴보자.

---
## 📍 atomic - 다른 컴포넌트에서 같은 기능 꺼내 쓰기

`Atomic` 구조를 구현하기 위해 `Question`기능은 한 폴더에 넣어두고 두 개의 서로 다른 컴포넌트에서 `Question` 기능을 꺼내 쓰려고 할 때, 다음과 같이 사용 할 수 있다.  컴포넌트를 이중으로 사용할 때는 \`children\`을 사용해서 내부 데이터까지 넘겨주는것을 잊지말자!!!

![](https://images.velog.io/images/abcd8637/post/c8fc0620-b581-4c8a-9421-ea102df6fdf2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-10%2021.42.48.png)

```javascript
// Question 기능
// src/components/Question/index.js

import styled from "styled-components";

const QuestionWrapper = styled.div`
  width: 400px;
`;

const QuestionLabel = styled.h1`
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 8px;
  text-align: center;
  color: grey;
`;

const QuestionTitle = styled.div`
  font-size: 22px;
  padding: 20px;
  margin-bottom: 8px;
  box-sizing: border-box;
`;

const Question = ({ QUIZ }) => (
  <QuestionWrapper>
    <QuestionLabel>
      <span>{QUIZ[0].id}</span>/{QUIZ.length}
    </QuestionLabel>
    <QuestionTitle>{QUIZ[0].question}</QuestionTitle>
  </QuestionWrapper>
);

export default Question;
```

```javascript
// Question 기능을 이용하는 Page1

import react from "react";
import Question from "../../components/Quiz/Question";

const Combat = ({ QUIZ, children }) => {
  return (
    <>
      <Question QUIZ={QUIZ}>{children}</Question>;
    </>
  );
};

export default Combat;
```

```javascript
// Question 기능을 이용하는 Page2

import react from "react";
import Question from "../../components/Quiz/Question";

const Supply = ({ QUIZ, children }) => {
  return (
    <>
      <Question QUIZ={QUIZ}>{children}</Question>;
    </>
  );
};

export default Supply;

```

```javascript
// App.js
import React from "react";
import Combat from "./pages/Combat";
import Supply from "./pages/Supply";
import { COMBAT_QUIZ } from "./pages/Combat/Constant";
import { SUPPLY_QUIZ } from "./pages/Supply/Constant";

const App = () => {
  return (
    <>
      <Combat QUIZ={COMBAT_QUIZ}></Combat>
      <Supply QUIZ={SUPPLY_QUIZ}></Supply>
    </>
  );
};

export default App;
```

## 💡 배운 점: 같은 기능을 구현하는 코드는 중복해서 쓰지말고 한곳에서 가져다 쓰자.


--- 
## 📍 props, onClick, handleClick 자세하게 이해하기

1. `Button` 컴포넌트를 모듈화(`module`)시킬 때 이름을 바로 정하지말고 `props`를 사용하여 위에서 값을 설정해서 바꿀 수 있게 끔 설정하자.
다음은 `Button` 컴포넌트를 외부에서 사용할 때 `props`로 `button`의 이름을 변경하는 간단한 기능이다.

```javascript
// components/App.js
const App = () => {
    return <Button text="IT IS BUTTON"></Button>;
};

// components/Button/index.js
const Button = (props) => {
  return <button>{props.text}</button>;
};

```

![](https://images.velog.io/images/abcd8637/post/7976f8a9-ccdb-4b2e-826b-d1c18d134c26/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-15%2015.56.56.png)

2. `Button` 컴포넌트에 `handleClick` 기능을 달아주고 싶을 때는 마찬가지로 `handleClick` 컴포넌트를 `props`로 내려주면 된다. 다만 `props`로 받고나서 `props.onClick()`처럼 괄호(`()`)를 사용해야 클릭했을 때 함수를 바로 실행할 수 있다. 괄호(`()`)를 넣지 않으면 오류가 난다.

```javascript
// src/components/Button/index.js
const Button = (props) => {
  return (
    <button
      onClick={() => {
        console.log(" i'm button. ");
        props.onClick();
      }}
    >
      {props.text}
    </button>
  );
};

// src/components/App.js
const App = () => {
    const handleClick = () => {
        console.log(" i'm handleClick!. ")
    }
  return <Button text="IT IS BUTTON" onClick={handleClick}></Button>;
};
```

![](https://images.velog.io/images/abcd8637/post/6677995d-d00f-4e3c-8f6a-7d0be4c56915/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-15%2016.02.00.png)

3. `handleClick`을 다른 함수에 넣어두고 그 함수를 적용하고 싶다면 다음과 같이 작성하면 된다. 이때에도 괄호(`()`)를 넣어주는것을 잊지말자!

```javascript
// src/components/Button/index.js
const Button = (props) => {
  return (
    <button
      onClick={() => {
        console.log(" i'm button. ");
        props.onClick();
      }}
    >
      {props.text}
    </button>
  );
};

// src/components/App.js
const App = () => {
    const handleClick = () => {
        console.log(" i'm handleClick!. ")
    }

    const handleClick2 = () => {
        handleClick();
    }

  return <Button text="IT IS BUTTON" onClick={handleClick2}></Button>;
};
```

![](https://images.velog.io/images/abcd8637/post/6677995d-d00f-4e3c-8f6a-7d0be4c56915/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-15%2016.02.00.png)

--- 
## 📍 함수로 전달해주는 인자가 3개 이상일 때, 객체로 묶어서 전달하기

함수로 넘겨주는 인자가 3개 이상이면, 객체 하나로 묶어 가독성이 좋아지게끔 하자. <a href ='https://github.com/qkraudghgh/clean-code-javascript-ko#%ED%95%A8%EC%88%98functions'>Robert C. Martin's - Clean Code 한글 번역판 함수(Functions)</a>

예를 들어, `defaultScore` 객체에 `str`, `dex`, `int`, `luck`의 능력치가 들어있는데 버튼을 클릭 할 때마다 능력치를 올라가게 설정하려고 한다면, 다음과 같이 작성 할 수 있다. 여기서 중요한 점은 `quizScore`에서 인자를 하나하나 따로 떼어서 받는게 아니라 전체를 묶어서 보낸다는 점이다. 이후에 함수 안에서 변수를 따로 설정하는 과정이다.

```javascript
// src/pages/score/Constant.js
const defaultScore = {
  stats: {
    str: 0,
    dex: 0,
    int: 0,
    luck: 0,
  },
};

// src/components/App.js
const App = () => {
  const [score, setScore] = useState(defaultScore);

  const quizScore = (defaultScore) => {
    const str = defaultScore.stats.str;
    const dex = defaultScore.stats.dex;
    const int = defaultScore.stats.int;
    const luck = defaultScore.stats.luck;

    setScore = (score) => ({
      ...score,
      stat: {
        strScore: score.stats.str + str,
        dexScore: score.stat.dex + dex,
        intScore: score.stat.int + int,
        luckScore: score.stat.luck + luck,
      },
    });
  };
};

```

---
## 📍 구조분해할당(Destructuring Assignment)으로 가독성 높이기

`react`의 코드를 리팩토링하던 중 `handleClick` 함수를 `module`화 시켜놓고 `handleClick`을 사용해야하는 각각의 `page`에서 다음과 같이 선언했었다.

`categoryHandleClick`의 `combat`, `supply`의 경우에 변수가 많지 않아 그러려니 할 수 있겠지만, `combatHandleClick`, `supplyHandleClick` 같은 경우 변수가 6개나 되기때문에 괜히 코드가 괜히 길어보이는 듯한 느낌을 받았다. 

이때 `구조분해할당(Destructuring Assignment)`을 사용해서 코드를 깔끔하게 줄일 수 있다. `구조분해할당(Destructuring Assignment)`을 잘 모르겠다면 다음 <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment'>MDN문서</a>를 확인해보자. 

아래의 코드는 구조분해할당을 하기 전의 코드이고 하단의 코드는 구조분해할당을 하고나서의 코드이다. 둘 중 어떤 코드가 가독성이 더 높아보이는가?! 사람마다 다를 수 있겠지만, 나는 하단의 코드가 더 가독성이 좋아보인다. 

```javascript
// Before Destructuring Assignment

// pages/category/index.js
const categoryHandleClick = (answer) => {
  const combat = answer.combat;
  const supply = answer.supply;

  setScore ((score) => ({
    ... skip code ...
  }))
}

// pages/combat/index.js
const combatHandleClick = (answer) => {
  const infantry = answer.infantry;
  const artillery = answer.artillery;
  const armor = answer.armor;
  const engineer = answer.engineer;
  const signal = answer.signal;
  const intelligence = answer.intelligence;

  setScore ((score) => ({
    ... skip code ...
  }))
}

// pages/supply/index.js
const supplyHandleClick = (answer) => {
  const affair = answer.affair;
  const medic = answer.medic;
  const weapon = answer.weapon;
  const police = answer.police;
  const pray = answer.pray;
  const band = answer.band;

  setScore ((score) => ({
    ... skip code ...
  }))
}
```

```javascript
// after Destructuring Assignment

// pages/category/index.js
const categoryHandleClick = ({ combat, supply }) => {
  setScore ((score) => ({
    ... skip code ...
  }));
};

// pages/combat/index.js
const combatHandleClick = ({ infantry, artillery, armor, engineer, signal, intelligence }) => {
  setScore ((score) => ({
    ... skip code ...
  }))
}

// pages/supply/index.js
const supplyHandleClick = ({ affair, medic, weapon, police, pray, band }) => {
  setScore ((score) => ({
    ... skip code ...
  }))
}
```

---
## 📍 객체 내의 객체를 구조분해할당(Double Destructuring Assignment) 하기

만약, `A`학생의 성적표와 기타 점수를 갖고있는 객체(Object)가 있다고 가정해보자. 그리고 `alert` 함수에서 `DEFAULT_SCORE`를 인자로 받고, 이때 `subjects`의 객체만을 구조분해할당을 하고 싶다면 다음과 같이 선언 할 수 있다.

```javascript
// @ param { Object }
const DEFAULT_SCORE = {
  subjects : {
    korean : 80,
    math : 85,
    english: 80
  },
  etc : {
    attitude: 90,
    volunteer: 95
  },
}


const alert = ( DEFAULT_SCORE ) => {
  const { subjects: {korean, math, english} } = DEFAULT_SCORE;
}
```

이전에 배웠던 <a href='https://ywtechit.tistory.com/132?category=936149'>구조분해할당</a>보다 난이도가 있어보이지만 구조를 들여다보면 금방 이해가 된다.

---
## 📍 구조분해할당시 변수 이름 변경하기

`구조분해할당(Destructuring_assignment)`이란 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 `JS` 표현식이다. (출처: <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment'>MDN</a>)

예를들어 `information` 객체(object)를 `alert` 함수에 인자(parameter)로 넘겨준다고 가정해보자. 그럼 다음과 같은 결과가 출력된다.

```javascript
// information Object
information = {
  name: 'AYW',
  age: 27,
  address: 'DongTan',
}

// alert Function
const alert = ({ name, age, address }) => {
  return {name, age, address}
}

// declare alert(information)
const myInformation = alert(information)
👉🏽 { name: 'AYW', age: 27, address: 'DongTan'}

```

여기서 비구조할당으로 넘어온 기존 변수를 새로운 이름으로 할당하려면 다음과 같이 사용 할 수 있다. 예를 들어 `name`을 `nickName`, `age`를 `number`, `address`를 `whereLive`로 변경해보자.

```javascript
// change parameter name at alert Function
const alert = ( information ) => {
  const { name: nickName, age: number, address: whereLive } = information;
  return { nickName, number, whereLive };
};

// declare alert(information)
const myInformation = alert(information)
👉🏽 { nickName: 'AYW', number: 27, whereLive: 'DongTan'}
```

이처럼 `현재 변수의 이름: 변경하고 싶은 이름 = parameter` 형태로 사용하면 변수의 이름을 손쉽게 변경 할 수 있다.

---
## 📍 Key:Value형태인 Object에 map 함수 사용하기

다음 `과목:점수` 형태인 `Object`에서 `map` 함수를 사용할때 3가지 방법이 있다. 

1. Object.entries: `array` 형태로 `key, value`를 반환한다. 주의 할 점은 반환시에 객체의 순서를 보장하지 않으므로 정렬을 먼저 하고나서 사용하는것을 권장한다. (`Object.entries(obj).sort((a, b) => b[0].localeCompare(a[0]));`, 출처: <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/entries'> MDN</a>)
2. Object.keys: `key`만 반환한다.
3. Object.values: `value`만 반환한다.

```javascript
const subjects = { math: 90, english: 100, science: 80 };

// idx는 index 번호를 반환하고 따로 명시하지 않았다.

const getEntries = Object.entries(subjects).map((entrie, idx) => {
  return console.log(entrie, idx);
});
👉🏽 ['math', 90], ['english', 100], ['science', 80]

const getKeys = Object.keys(subjects).map((entrie, idx) => {
  return console.log(entrie, idx);
});
👉🏽 math, english, science

const getValues = Object.values(subjects).map((entrie, idx) => {
  return console.log(entrie, idx);
});
👉🏽 90, 100, 80
```

---
## 📍 console 창에서 Warning: Using UNSAFE_componentWillMount in strict mode is not recommended and may indicate bugs in your code 뜰 때

<a href='https://ywtechmilitarytest.site/'>병과 테스트</a> 프로젝트를 잘 진행하고 있다가 갑자기 `console` 창에 다음 사진과 같은 `Warning`이 떴다.

![](https://images.velog.io/images/abcd8637/post/3405476a-4c37-48f4-91a3-8e1283457b11/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-06-15%2014.38.45.png)

구글링을 해보니까 <a href='https://github.com/nfl/react-helmet/issues/548'>nfl/react-helmet#548</a>, <a href='https://github.com/nfl/react-helmet/issues/623'>nfl/react-helmet#623</a> 에 비슷한 글이 있었는데 원인은 `strict mode`에서 `UNSAFE_componentWillMount`를 권장하지 않고 가끔 버그를 일으킬 수 있다는 내용이었다.

해결방법을 찾아보니 보통 `React-helmet` 버전이 `^6.0.0` 이하에서 많이 발생한다는 내용이 대다수였다. 하지만 내가 설치한 `React-helmet` 버전은 `^6.1.0`이었고, 버전이 낮아서 발생한것 같진 않아보였다. (혹시 버전이 `^6.0.0`이하라면, 라이브러리 업데이트를 하고나서 `import` 할 때 `import { Helmet } from "react-helmet"`로 작성해보자.)

다른 글 중에 `react-helmet-async`를 사용해보라는 글이 있어서 해당 라이브러리를 다운받고 다음과 같이 작성했더니 더 이상 `warning` 창이 뜨지 않았다. 깃허브에서 <a href= 'https://github.com/staylor/react-helmet-async'>react-helmet-async</a>패키지에 들어가보니까 `Helmet`과 사용법은 동일하고 스레드가 안전하지 않은 `react-side-effect`에 의존한다고 나와있다. 사용목적은 서버에서 비동기 작업을 수행 할 경우 데이터 요청별로 `Helmet`을 캡슐화 하기위해 사용하는 패키지라고 나와있다. 그냥 `Helmet`을 캡슐화 해준다고 생각하면 편하다.

사용법은 다음 코드를 참고하자.

```javascript
import { Helmet, HelmetProvider } from "react-helmet-async";

const Landing = () => {
  return (
    <>
      <HelmetProvider>
        <Helmet>
          {/* contents... */}
        </Helmet>
      </HelmetProvider>
    </>
  )
}

export default Landing;
```

![](https://images.velog.io/images/abcd8637/post/fd2c1a0b-8625-47f7-ba88-c1c794f6e09d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-06-15%2014.52.18.png)

---
## 📍 Object value에 NaN값이 있는지 확인하는 함수 만들기
처음에 `NaN` 값을 확인하기 위해 `in`, `===`, `!=`를 사용했는데 판별이 되지 않았다. 
왜그런지 <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/NaN'>MDN</a>공식문서에서 찾아보니까 `NaN` 판별은 비교판별 대신 `isNaN()` 혹은 `Number.isNaN()` 내장함수로 판별해야한다고 나와있었다. (⭐️ 공식문서의 중요성 ⭐️)

`isNaN`, `Number.isNaN()`은 둘다 `NaN`을 판별하는 함수이지만 순서가 조금 다르다. 

1. `isNaN()`: 현재 값을 숫자로 변환하고 `NaN` 판별
2. `Number.isNaN()`: 현재 `typeof => Number` && 값이 `NaN` 일때만 판별

이를 바탕으로 `key:value` 형태로 이루어진 객체에서 `isNaN`, `Number.isNaN()` 함수를 사용하니까 정확한 판별이 이루어지지 않았다. 왜냐하면 `object` 값을 `Number` 형태로 변환하면 `NaN`이 나오기 때문이다. 이 상태에서 `NaN` 함수 판별을 하게되면 `NaN`의 유무와 상관없이 `true`가 `return` 된다.

```javascript
const myObject = {name: '안영우', age: 27, sex: NaN}
const yourObject = {name: '안영준', age: 14, sex: 'female'}

console.log(Number(myObject))
👉🏽 NaN

console.log(Number(yourObject))
👉🏽 NaN
```

그래서 `isNaN`, `Number.isNaN()` 함수를 사용하는 대신 `array`값에서 `NaN`이 있는지 확인하는 `includes()` 함수를 사용했다.

1. Object의 Value만을 뽑아 `array(배열)`로 만든다.
2. <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/includes'>includes()</a> 메소드를 이용해 배열이 `NaN`값을 포함하고 있는지 확인한다.

```javascript
const myObject = {name: '안영우', age: 27, sex: NaN}
const yourObject = {name: '안영준', age: 14, sex: 'female'}

const getCheckNaN = (object) => {
    let arrayValues =  Object.values(object);
    let current;

    if (arrayValues.includes(NaN)){
        current = true
    }else{
        current = false
    }
    return current
}

console.log(getCheckNaN(myInfo))
👉🏽 true

console.log(getCheckNaN(yourInfo))
👉🏽 false
```

---
## 📍 console 창 Received `true` for a non-boolean attribute 혹은 Received `true` for a non-boolean attribute 해결법
<a href='https://ywtechmilitarytest.site/'>병과 테스트 프로젝트</a> 에서 `styled-component`로 `image-blur-up` 기능을 구현하던 도중 `console.log`창에 다음과 같은 `warning`이 나타난 적 있다.

![](https://images.velog.io/images/abcd8637/post/fc214fc9-1d40-4f79-9752-354fefb6c492/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-02%2013.47.17.png)

---
## 📍 Uncaught SyntaxError: Unexpected token '<’ 오류 해결하기
사이트에 배포하기 위해 `Webpack`을 사용하여 `npm run build` 실행 후 `/dist`에 있는 `index.html`파일을 실행해봤는데 정상적으로 페이지가 나타나지 않아 `console`창을 열어보니 `Uncaught SyntaxError: Unexpected token '<’`에러가 나타났다. 해결방법을 위해 구글링을 하다 `GitHub`에 동일한 <a href='https://github.com/webpack/webpack/issues/2882'>이슈</a>가 있었다. 원인으로는 `react-router`는 `js`파일이 필요했지만, `html`파일을 응답으로 줬기때문에 오류가 생긴것이었다. 결론적으로 `index.html`파일의 `<head></head>`태그 사이에 `<base href="/" />` 코드를 추가하니까 해결되었다.

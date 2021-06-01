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
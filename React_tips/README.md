## π λΆνμν prop drilling μ κ±°νκΈ°
`React`μμ νμ΄μ§ λ΄λΆμ 2~3κ°μ μ»΄ν¬λνΈλ₯Ό μ μΈνλ©° `props`λ‘ μ λ¬νλ κ²½μ°κ° μμ κ²μ΄λ€. μ΄λ² κΈμ ν νμ΄μ§ λ΄μ μ»΄ν¬λνΈ 3κ°(`A` -> `B` -> `C`)λ₯Ό μ μΈνμ¬ λ¨μν `props`λ‘ μ λ¬λ§νλ `B`μ»΄ν¬λνΈμ `prop drilling`μ μ΄λ»κ² ν΄κ²°νλμ§ μμλ³΄μ.

μ°μ , `React`λ₯Ό λ€λ€ λ³Έ κ°λ°μλΌλ©΄ `prop drilling`μ λν΄ ν λ²μ―€μ λ€μ΄λ΄€μ κ²μ΄λ€. `prop drilling`μ΄λ, `props`λ₯Ό μ΄μ©ν΄ μμ μ»΄ν¬λνΈλ‘ λ°μ΄ν°λ₯Ό λ΄λ €μ€ λ λͺ¨λ  λ λ²¨μμ κ°μ λ°μ΄ν°κ° μ μ‘λλ μν©μ λ§νλ€.

![](https://velog.velcdn.com/images/abcd8637/post/fa003998-d2fb-40db-b998-cc496bef0acf/image.png)

κ·λͺ¨κ° μμ λ `prop drilling`μ λΉ λ₯΄κ³  μ½κ² λ°μ΄ν°λ₯Ό μ μ‘ν  μ μκ³ , κ΅¬ν λ°©λ²μ΄ μ½κ³ , `props`λ‘ μ λ¬λ λ°μ΄ν°μ μν λ³κ²½ μ μλ‘μ΄ λ³κ²½μ¬ν­μ μ½κ² μλ°μ΄νΈ ν  μ μλ μ₯μ μ΄ μλ€. κ·Έλ¬λ μμ μ»΄ν¬λνΈκ° λΆλͺ¨ μ»΄ν¬λνΈμ λ°μ΄ν°λ₯Ό μ λ¬ν΄μ£Όλ μ­ν λ‘λ§ μ¬μ©λκ³  κ·Έ λλ¬Έμ μ½λκ° λΆνμνκ² κΈΈμ΄μ§λ©° νμνμ§ μμ λ°μ΄ν°κΉμ§ μ λ¬ν΄μ€ μ μκΈ° λλ¬Έμ νλ‘μ νΈμ κ·λͺ¨κ° μ»€μ§μλ‘ κΆμ₯νμ§ μλ λ°©λ²μ΄λ€. 

μ΄λ΄ κ²½μ° `contextAPI` νΉμ μ μ­μνκ΄λ¦¬ λΌμ΄λΈλ¬λ¦¬μ λμμ λ°μ μ μμ§λ§, μ΄λ²μ κ·Έλ€μ λμμ λ°μ§ μκ³  μ½λλ₯Ό μ΄λ»κ² μ€μλμ§λ₯Ό μμλ₯Ό λ€μ΄ μ΄ν΄λ³΄μ.

μ°μ , `API`λ₯Ό μ΄μ©ν΄ `User`κ° κ°μν κ³μ μ μ°Ύλ `Accounts` λ°μ΄ν°λ₯Ό κ°μ Έμ¨λ€κ³  κ°μ ν΄λ³΄μ. 

νμλ μ²μ `AccountsFound`λ μ»΄ν¬λνΈμμ `User`μ `Accounts`λ₯Ό κ°μ Έμ¨λ€. κ·Έλ¦¬κ³  κ°μ λ°μ΄ν°μ `Accounts`κ° λ΄κ²¨μλ λ°μ΄ν°λ₯Ό `Accounts`λ μ»΄ν¬λνΈλ‘ λκΈ°κ³  μ΄λ€μ νλμ© λ³΄μ¬μ£ΌκΈ° μν΄ `map`ν¨μλ₯Ό μ¬μ©ν΄ `Account`λΌλ μ»΄ν¬λνΈλ‘ μ λ¬ν΄μ£Όμλ€. μλ μ½λλΈλ‘μ λ³΄μ.

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
      <div>κ³ κ°λμ κ³μ  μ λ³΄μλλ€.</div>
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

μ΄ νμ΄μ§μμ μμ±λ μ½λ μ€ λΉν¨μ¨μ μΌλ‘ μμ±λ λΆλΆμ μ°Ύμ μ μκ² λκ°?

1. 15λΌμΈμ `<div>`λ `map`μ ν΅ν΄ `accounts`μ κ°μλ§νΌ μλ‘­κ² λ λλ§ λλ€. κ°μ λ°μ΄ν°λ₯Ό λΆνμνκ² λ§€λ² μλ‘­κ² λ λλ§ λλ€.
2. λΆλͺ¨(`<Accounts />`) μ»΄ν¬λνΈλ μμ(`<Account />`) μ»΄ν¬λνΈμκ² λ¨μν λ°μ΄ν°λ§ μ λ¬ν΄μ£Όλ μ­ν λ§ νκ³  μλ€. (μ΄ λΆλΆμ΄ `propDrilling`) λ°λΌμ, `<Accounts />` μ»΄ν¬λνΈλ₯Ό μμ€λ€. μ¦, μ»΄ν¬λνΈμ λ λλ§ μμλ₯Ό `AccountsFound -> Accounts -> Account`μμ `AccountsFound -> Account`λ‘ μ€μ¬μ€λ€.

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
      <div>κ³ κ°λμ κ³μ  μ λ³΄μλλ€.</div>
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

μ΄λ κ² ν΄μ `prop drilling`μ μ μ­μνκ΄λ¦¬λ‘ ν΄κ²°νλ κ²μ΄ μλλΌ λΆνμν μ»΄ν¬λνΈλ₯Ό μ κ±°νλ©΄μ μ½λλ₯Ό κ°λ¨ν μ€μ΄λ μμλ₯Ό μ΄ν΄λ΄€λ€. λ¨μν λ°μ΄ν°λ§ μ λ¬ν΄μ£Όλ μ»΄ν¬λνΈκ° λ§μμ‘λ€κ³  μκ°νλ©΄ `prop drilling`μ νλ κ²μ μλμ§ ν λ²μ―€ μμ μ λλμλ³Ό μ μλ μκ°μ μ κΉμ΄λ κ°μ§λ©° μ΄ κΈμ λ§μΉλ€. 

Reference
1. https://www.geeksforgeeks.org/what-is-prop-drilling-and-how-to-avoid-it/

---
## π Routeμμ props μ λ¬μ΄ μλ  λ

React Routerλ₯Ό μ¬μ©ν  λ `props`κ° μ μμ μΌλ‘ μ λ¬λμ§ μμΌλ©΄ μ΄λ€ `component`μ `props`λ₯Ό μ£Όκ³ μλμ§ νμΈν΄λ³΄μ.
λλ 1λ²κ³Ό κ°μ΄ μ½λλ₯Ό κ°κ²°νκ² μμ±νλ €κ³  ν μ€λ‘ μμ±νλ€κ° μ μλλ νλλ `Route`μ»΄ν¬λνΈμ `props`λ₯Ό μ λ¬ν΄μ£Όλ μ½λκΈ° λλ¬Έμ΄λ€.

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

1.  1λ² λ°©λ²μΌλ‘ μμ±νμ λ  
    
    ![](https://images.velog.io/images/abcd8637/post/1e31a5e8-74d4-42a4-89cd-df35ae506fcb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-28%2016.50.29.png)
    
2.  2λ² λ°©λ²μΌλ‘ μμ±νμ λ  
    
    ![](https://images.velog.io/images/abcd8637/post/9d3704b9-0045-42a8-8af3-ba1d305bb6af/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-04-28%2016.47.45.png)
    
---
## π propsλ‘ μ λ¬ν νλΌλ―Έν°κ° μ μμ μΌλ‘ ν¨μμ μ μ©λμ§ μμ λ

`props`λ‘ μ λ¬λ°μ κ°λ€μ μλ‘μ΄ ν¨μλ₯Ό κ±°μ³μ λμ¬ λ μλν λλ‘ λμνμ§ μμΌλ©΄ λ€μμ νμΈν΄λ³΄μ.

1.  ν¨μ μμ `props`λ₯Ό λ°λ‘ μ μ©νμ§ μμλ? - (X)
2.  μ»΄ν¬λνΈ μμ ν¨μλ₯Ό μ μΈνκ³  `props`λ‘ λ°μ κ°λ€μ νλΌλ―Έν°λ‘ λ£μλκ°? - (O)

## π‘ λ°°μ΄ μ : νμ€νΈμ½λμ μ€μμ±

μ΄μ κ°μ λ¬Έμ κ° μκΈ°μ§ μκΈ° μν΄ κΈ°λ₯λ³λ‘ μλΌ λ¨κ³μ μΌλ‘ μ€νν΄λ³΄λ©΄ μ€ν¨λ₯Ό μ€μΌ μ μλ€.

1.  ν΄λΉ ν¨μλ§ λ°λ‘ `console.log`μ μ?κ²¨μ λ΄λΆ κΈ°λ₯μ λ¨Όμ  νμ€νΈν΄λ³Έλ€.
2.  ν¨μ λ΄λΆμ μΌλ‘ λ¬Έμ κ° μλ€λ©΄ `props`μμ λ°μ λ³μλ₯Ό ν¨μμ λ£μ΄λ³Έλ€.
3.  μ΄λμμ μλͺ»λλμ§ νμΈνλ€.

```javascript
// κ³΅ν΅ μ€νμ½λ
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
## π λͺ¨λ°μΌνλ©΄μ΄ λ²μ΄λ  λ

## β‘οΈ κ΅¬ν ν

μΉνμ΄μ§μμλ λ΄κ° μ€μ ν νλ©΄ κ·Έλλ‘ μ λμ€λλ° λͺ¨λ°μΌνλ©΄μμλ νλ©΄μ΄ κΉ¨μ§κ±°λ λ²μ΄λ  λ

1.  index pageμμ `viewport` μ½λκ° μ μλκ°?

---

## π‘ λ°°μ΄ μ : μ½λλ₯Ό μ§μΈ λλ μ μ§μ°λμ§ μκ°νμ.

`React Hemlet`μ μ¬μ©νκΈ°μν΄ `public`μμμλ `index.html`λ΄μ©μ λͺ¨λ μ§μ°κ³  `App.js`μ μλ‘ μμ±νμ§λ§, κ²°κ³Όμ μΌλ‘ `<meta name="viewport" content="width=device-width, initial-scale=1" />` ν΄λΉ μ½λκ° μμ΄ λͺ¨λ°μΌνλ©΄μ μ μμ μΌλ‘ μΆλ ₯λμ§ μμλ€. λ§μ½ μ§κΈμ μν©κ³Ό λΉμ·νλ€λ©΄ `index,html`νμ΄μ§λ₯Ό νλ² μ΄ν΄λ³΄μ.

---
## π atomic - λ€λ₯Έ μ»΄ν¬λνΈμμ κ°μ κΈ°λ₯ κΊΌλ΄ μ°κΈ°

`Atomic` κ΅¬μ‘°λ₯Ό κ΅¬ννκΈ° μν΄ `Question`κΈ°λ₯μ ν ν΄λμ λ£μ΄λκ³  λ κ°μ μλ‘ λ€λ₯Έ μ»΄ν¬λνΈμμ `Question` κΈ°λ₯μ κΊΌλ΄ μ°λ €κ³  ν  λ, λ€μκ³Ό κ°μ΄ μ¬μ© ν  μ μλ€.Β  μ»΄ν¬λνΈλ₯Ό μ΄μ€μΌλ‘ μ¬μ©ν  λλ \`children\`μ μ¬μ©ν΄μ λ΄λΆ λ°μ΄ν°κΉμ§ λκ²¨μ£Όλκ²μ μμ§λ§μ!!!

![](https://images.velog.io/images/abcd8637/post/c8fc0620-b581-4c8a-9421-ea102df6fdf2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-10%2021.42.48.png)

```javascript
// Question κΈ°λ₯
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
// Question κΈ°λ₯μ μ΄μ©νλ Page1

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
// Question κΈ°λ₯μ μ΄μ©νλ Page2

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

## π‘ λ°°μ΄ μ : κ°μ κΈ°λ₯μ κ΅¬ννλ μ½λλ μ€λ³΅ν΄μ μ°μ§λ§κ³  νκ³³μμ κ°μ Έλ€ μ°μ.


--- 
## π props, onClick, handleClick μμΈνκ² μ΄ν΄νκΈ°

1. `Button` μ»΄ν¬λνΈλ₯Ό λͺ¨λν(`module`)μν¬ λ μ΄λ¦μ λ°λ‘ μ νμ§λ§κ³  `props`λ₯Ό μ¬μ©νμ¬ μμμ κ°μ μ€μ ν΄μ λ°κΏ μ μκ² λ μ€μ νμ.
λ€μμ `Button` μ»΄ν¬λνΈλ₯Ό μΈλΆμμ μ¬μ©ν  λ `props`λ‘ `button`μ μ΄λ¦μ λ³κ²½νλ κ°λ¨ν κΈ°λ₯μ΄λ€.

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

2. `Button` μ»΄ν¬λνΈμ `handleClick` κΈ°λ₯μ λ¬μμ£Όκ³  μΆμ λλ λ§μ°¬κ°μ§λ‘ `handleClick` μ»΄ν¬λνΈλ₯Ό `props`λ‘ λ΄λ €μ£Όλ©΄ λλ€. λ€λ§ `props`λ‘ λ°κ³ λμ `props.onClick()`μ²λΌ κ΄νΈ(`()`)λ₯Ό μ¬μ©ν΄μΌ ν΄λ¦­νμ λ ν¨μλ₯Ό λ°λ‘ μ€νν  μ μλ€. κ΄νΈ(`()`)λ₯Ό λ£μ§ μμΌλ©΄ μ€λ₯κ° λλ€.

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

3. `handleClick`μ λ€λ₯Έ ν¨μμ λ£μ΄λκ³  κ·Έ ν¨μλ₯Ό μ μ©νκ³  μΆλ€λ©΄ λ€μκ³Ό κ°μ΄ μμ±νλ©΄ λλ€. μ΄λμλ κ΄νΈ(`()`)λ₯Ό λ£μ΄μ£Όλκ²μ μμ§λ§μ!

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
## π ν¨μλ‘ μ λ¬ν΄μ£Όλ μΈμκ° 3κ° μ΄μμΌ λ, κ°μ²΄λ‘ λ¬Άμ΄μ μ λ¬νκΈ°

ν¨μλ‘ λκ²¨μ£Όλ μΈμκ° 3κ° μ΄μμ΄λ©΄, κ°μ²΄ νλλ‘ λ¬Άμ΄ κ°λμ±μ΄ μ’μμ§κ²λ νμ. <a href ='https://github.com/qkraudghgh/clean-code-javascript-ko#%ED%95%A8%EC%88%98functions'>Robert C. Martin's - Clean Code νκΈ λ²μ­ν ν¨μ(Functions)</a>

μλ₯Ό λ€μ΄, `defaultScore` κ°μ²΄μ `str`, `dex`, `int`, `luck`μ λ₯λ ₯μΉκ° λ€μ΄μλλ° λ²νΌμ ν΄λ¦­ ν  λλ§λ€ λ₯λ ₯μΉλ₯Ό μ¬λΌκ°κ² μ€μ νλ €κ³  νλ€λ©΄, λ€μκ³Ό κ°μ΄ μμ± ν  μ μλ€. μ¬κΈ°μ μ€μν μ μ `quizScore`μμ μΈμλ₯Ό νλνλ λ°λ‘ λΌμ΄μ λ°λκ² μλλΌ μ μ²΄λ₯Ό λ¬Άμ΄μ λ³΄λΈλ€λ μ μ΄λ€. μ΄νμ ν¨μ μμμ λ³μλ₯Ό λ°λ‘ μ€μ νλ κ³Όμ μ΄λ€.

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
## π κ΅¬μ‘°λΆν΄ν λΉ(Destructuring Assignment)μΌλ‘ κ°λμ± λμ΄κΈ°

`react`μ μ½λλ₯Ό λ¦¬ν©ν λ§νλ μ€ `handleClick` ν¨μλ₯Ό `module`ν μμΌλκ³  `handleClick`μ μ¬μ©ν΄μΌνλ κ°κ°μ `page`μμ λ€μκ³Ό κ°μ΄ μ μΈνμλ€.

`categoryHandleClick`μ `combat`, `supply`μ κ²½μ°μ λ³μκ° λ§μ§ μμ κ·Έλ¬λ €λ ν  μ μκ² μ§λ§, `combatHandleClick`, `supplyHandleClick` κ°μ κ²½μ° λ³μκ° 6κ°λ λκΈ°λλ¬Έμ κ΄ν μ½λκ° κ΄ν κΈΈμ΄λ³΄μ΄λ λ―ν λλμ λ°μλ€. 

μ΄λ `κ΅¬μ‘°λΆν΄ν λΉ(Destructuring Assignment)`μ μ¬μ©ν΄μ μ½λλ₯Ό κΉλνκ² μ€μΌ μ μλ€. `κ΅¬μ‘°λΆν΄ν λΉ(Destructuring Assignment)`μ μ λͺ¨λ₯΄κ² λ€λ©΄ λ€μ <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment'>MDNλ¬Έμ</a>λ₯Ό νμΈν΄λ³΄μ. 

μλμ μ½λλ κ΅¬μ‘°λΆν΄ν λΉμ νκΈ° μ μ μ½λμ΄κ³  νλ¨μ μ½λλ κ΅¬μ‘°λΆν΄ν λΉμ νκ³ λμμ μ½λμ΄λ€. λ μ€ μ΄λ€ μ½λκ° κ°λμ±μ΄ λ λμλ³΄μ΄λκ°?! μ¬λλ§λ€ λ€λ₯Ό μ μκ² μ§λ§, λλ νλ¨μ μ½λκ° λ κ°λμ±μ΄ μ’μλ³΄μΈλ€. 

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
## π κ°μ²΄ λ΄μ κ°μ²΄λ₯Ό κ΅¬μ‘°λΆν΄ν λΉ(Double Destructuring Assignment) νκΈ°

λ§μ½, `A`νμμ μ±μ νμ κΈ°ν μ μλ₯Ό κ°κ³ μλ κ°μ²΄(Object)κ° μλ€κ³  κ°μ ν΄λ³΄μ. κ·Έλ¦¬κ³  `alert` ν¨μμμ `DEFAULT_SCORE`λ₯Ό μΈμλ‘ λ°κ³ , μ΄λ `subjects`μ κ°μ²΄λ§μ κ΅¬μ‘°λΆν΄ν λΉμ νκ³  μΆλ€λ©΄ λ€μκ³Ό κ°μ΄ μ μΈ ν  μ μλ€.

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

μ΄μ μ λ°°μ λ <a href='https://ywtechit.tistory.com/132?category=936149'>κ΅¬μ‘°λΆν΄ν λΉ</a>λ³΄λ€ λμ΄λκ° μμ΄λ³΄μ΄μ§λ§ κ΅¬μ‘°λ₯Ό λ€μ¬λ€λ³΄λ©΄ κΈλ°© μ΄ν΄κ° λλ€.

---
## π κ΅¬μ‘°λΆν΄ν λΉμ λ³μ μ΄λ¦ λ³κ²½νκΈ°

`κ΅¬μ‘°λΆν΄ν λΉ(Destructuring_assignment)`μ΄λ λ°°μ΄μ΄λ κ°μ²΄μ μμ±μ ν΄μ²΄νμ¬ κ·Έ κ°μ κ°λ³ λ³μμ λ΄μ μ μκ² νλ `JS` ννμμ΄λ€. (μΆμ²: <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment'>MDN</a>)

μλ₯Όλ€μ΄ `information` κ°μ²΄(object)λ₯Ό `alert` ν¨μμ μΈμ(parameter)λ‘ λκ²¨μ€λ€κ³  κ°μ ν΄λ³΄μ. κ·ΈλΌ λ€μκ³Ό κ°μ κ²°κ³Όκ° μΆλ ₯λλ€.

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
ππ½ { name: 'AYW', age: 27, address: 'DongTan'}

```

μ¬κΈ°μ λΉκ΅¬μ‘°ν λΉμΌλ‘ λμ΄μ¨ κΈ°μ‘΄ λ³μλ₯Ό μλ‘μ΄ μ΄λ¦μΌλ‘ ν λΉνλ €λ©΄ λ€μκ³Ό κ°μ΄ μ¬μ© ν  μ μλ€. μλ₯Ό λ€μ΄ `name`μ `nickName`, `age`λ₯Ό `number`, `address`λ₯Ό `whereLive`λ‘ λ³κ²½ν΄λ³΄μ.

```javascript
// change parameter name at alert Function
const alert = ( information ) => {
  const { name: nickName, age: number, address: whereLive } = information;
  return { nickName, number, whereLive };
};

// declare alert(information)
const myInformation = alert(information)
ππ½ { nickName: 'AYW', number: 27, whereLive: 'DongTan'}
```

μ΄μ²λΌ `νμ¬ λ³μμ μ΄λ¦: λ³κ²½νκ³  μΆμ μ΄λ¦ = parameter` ννλ‘ μ¬μ©νλ©΄ λ³μμ μ΄λ¦μ μμ½κ² λ³κ²½ ν  μ μλ€.

---
## π Key:ValueννμΈ Objectμ map ν¨μ μ¬μ©νκΈ°

λ€μ `κ³Όλͺ©:μ μ` ννμΈ `Object`μμ `map` ν¨μλ₯Ό μ¬μ©ν λ 3κ°μ§ λ°©λ²μ΄ μλ€. 

1. Object.entries: `array` ννλ‘ `key, value`λ₯Ό λ°ννλ€. μ£Όμ ν  μ μ λ°νμμ κ°μ²΄μ μμλ₯Ό λ³΄μ₯νμ§ μμΌλ―λ‘ μ λ ¬μ λ¨Όμ  νκ³ λμ μ¬μ©νλκ²μ κΆμ₯νλ€. (`Object.entries(obj).sort((a, b) => b[0].localeCompare(a[0]));`, μΆμ²: <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/entries'> MDN</a>)
2. Object.keys: `key`λ§ λ°ννλ€.
3. Object.values: `value`λ§ λ°ννλ€.

```javascript
const subjects = { math: 90, english: 100, science: 80 };

// idxλ index λ²νΈλ₯Ό λ°ννκ³  λ°λ‘ λͺμνμ§ μμλ€.

const getEntries = Object.entries(subjects).map((entrie, idx) => {
  return console.log(entrie, idx);
});
ππ½ ['math', 90], ['english', 100], ['science', 80]

const getKeys = Object.keys(subjects).map((entrie, idx) => {
  return console.log(entrie, idx);
});
ππ½ math, english, science

const getValues = Object.values(subjects).map((entrie, idx) => {
  return console.log(entrie, idx);
});
ππ½ 90, 100, 80
```

---
## π console μ°½μμ Warning: Using UNSAFE_componentWillMount in strict mode is not recommended and may indicate bugs in your code λ° λ

<a href='https://ywtechmilitarytest.site/'>λ³κ³Ό νμ€νΈ</a> νλ‘μ νΈλ₯Ό μ μ§ννκ³  μλ€κ° κ°μκΈ° `console` μ°½μ λ€μ μ¬μ§κ³Ό κ°μ `Warning`μ΄ λ΄λ€.

![](https://images.velog.io/images/abcd8637/post/3405476a-4c37-48f4-91a3-8e1283457b11/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-06-15%2014.38.45.png)

κ΅¬κΈλ§μ ν΄λ³΄λκΉ <a href='https://github.com/nfl/react-helmet/issues/548'>nfl/react-helmet#548</a>, <a href='https://github.com/nfl/react-helmet/issues/623'>nfl/react-helmet#623</a> μ λΉμ·ν κΈμ΄ μμλλ° μμΈμ `strict mode`μμ `UNSAFE_componentWillMount`λ₯Ό κΆμ₯νμ§ μκ³  κ°λ λ²κ·Έλ₯Ό μΌμΌν¬ μ μλ€λ λ΄μ©μ΄μλ€.

ν΄κ²°λ°©λ²μ μ°Ύμλ³΄λ λ³΄ν΅ `React-helmet` λ²μ μ΄ `^6.0.0` μ΄νμμ λ§μ΄ λ°μνλ€λ λ΄μ©μ΄ λλ€μμλ€. νμ§λ§ λ΄κ° μ€μΉν `React-helmet` λ²μ μ `^6.1.0`μ΄μκ³ , λ²μ μ΄ λ?μμ λ°μνκ² κ°μ§ μμλ³΄μλ€. (νΉμ λ²μ μ΄ `^6.0.0`μ΄νλΌλ©΄, λΌμ΄λΈλ¬λ¦¬ μλ°μ΄νΈλ₯Ό νκ³ λμ `import` ν  λ `import { Helmet } from "react-helmet"`λ‘ μμ±ν΄λ³΄μ.)

λ€λ₯Έ κΈ μ€μ `react-helmet-async`λ₯Ό μ¬μ©ν΄λ³΄λΌλ κΈμ΄ μμ΄μ ν΄λΉ λΌμ΄λΈλ¬λ¦¬λ₯Ό λ€μ΄λ°κ³  λ€μκ³Ό κ°μ΄ μμ±νλλ λ μ΄μ `warning` μ°½μ΄ λ¨μ§ μμλ€. κΉνλΈμμ <a href= 'https://github.com/staylor/react-helmet-async'>react-helmet-async</a>ν¨ν€μ§μ λ€μ΄κ°λ³΄λκΉ `Helmet`κ³Ό μ¬μ©λ²μ λμΌνκ³  μ€λ λκ° μμ νμ§ μμ `react-side-effect`μ μμ‘΄νλ€κ³  λμμλ€. μ¬μ©λͺ©μ μ μλ²μμ λΉλκΈ° μμμ μν ν  κ²½μ° λ°μ΄ν° μμ²­λ³λ‘ `Helmet`μ μΊ‘μν νκΈ°μν΄ μ¬μ©νλ ν¨ν€μ§λΌκ³  λμμλ€. κ·Έλ₯ `Helmet`μ μΊ‘μν ν΄μ€λ€κ³  μκ°νλ©΄ νΈνλ€.

μ¬μ©λ²μ λ€μ μ½λλ₯Ό μ°Έκ³ νμ.

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
## π Object valueμ NaNκ°μ΄ μλμ§ νμΈνλ ν¨μ λ§λ€κΈ°
μ²μμ `NaN` κ°μ νμΈνκΈ° μν΄ `in`, `===`, `!=`λ₯Ό μ¬μ©νλλ° νλ³μ΄ λμ§ μμλ€. 
μκ·Έλ°μ§ <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/NaN'>MDN</a>κ³΅μλ¬Έμμμ μ°Ύμλ³΄λκΉ `NaN` νλ³μ λΉκ΅νλ³ λμ  `isNaN()` νΉμ `Number.isNaN()` λ΄μ₯ν¨μλ‘ νλ³ν΄μΌνλ€κ³  λμμμλ€. (β­οΈ κ³΅μλ¬Έμμ μ€μμ± β­οΈ)

`isNaN`, `Number.isNaN()`μ λλ€ `NaN`μ νλ³νλ ν¨μμ΄μ§λ§ μμκ° μ‘°κΈ λ€λ₯΄λ€. 

1. `isNaN()`: νμ¬ κ°μ μ«μλ‘ λ³ννκ³  `NaN` νλ³
2. `Number.isNaN()`: νμ¬ `typeof => Number` && κ°μ΄ `NaN` μΌλλ§ νλ³

μ΄λ₯Ό λ°νμΌλ‘ `key:value` ννλ‘ μ΄λ£¨μ΄μ§ κ°μ²΄μμ `isNaN`, `Number.isNaN()` ν¨μλ₯Ό μ¬μ©νλκΉ μ νν νλ³μ΄ μ΄λ£¨μ΄μ§μ§ μμλ€. μλνλ©΄ `object` κ°μ `Number` ννλ‘ λ³ννλ©΄ `NaN`μ΄ λμ€κΈ° λλ¬Έμ΄λ€. μ΄ μνμμ `NaN` ν¨μ νλ³μ νκ²λλ©΄ `NaN`μ μ λ¬΄μ μκ΄μμ΄ `true`κ° `return` λλ€.

```javascript
const myObject = {name: 'μμμ°', age: 27, sex: NaN}
const yourObject = {name: 'μμμ€', age: 14, sex: 'female'}

console.log(Number(myObject))
ππ½ NaN

console.log(Number(yourObject))
ππ½ NaN
```

κ·Έλμ `isNaN`, `Number.isNaN()` ν¨μλ₯Ό μ¬μ©νλ λμ  `array`κ°μμ `NaN`μ΄ μλμ§ νμΈνλ `includes()` ν¨μλ₯Ό μ¬μ©νλ€.

1. Objectμ Valueλ§μ λ½μ `array(λ°°μ΄)`λ‘ λ§λ λ€.
2. <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/includes'>includes()</a> λ©μλλ₯Ό μ΄μ©ν΄ λ°°μ΄μ΄ `NaN`κ°μ ν¬ν¨νκ³  μλμ§ νμΈνλ€.

```javascript
const myObject = {name: 'μμμ°', age: 27, sex: NaN}
const yourObject = {name: 'μμμ€', age: 14, sex: 'female'}

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
ππ½ true

console.log(getCheckNaN(yourInfo))
ππ½ false
```

---
## π console μ°½ Received `true` for a non-boolean attribute νΉμ Received `true` for a non-boolean attribute ν΄κ²°λ²
<a href='https://ywtechmilitarytest.site/'>λ³κ³Ό νμ€νΈ νλ‘μ νΈ</a> μμ `styled-component`λ‘ `image-blur-up` κΈ°λ₯μ κ΅¬ννλ λμ€ `console.log`μ°½μ λ€μκ³Ό κ°μ `warning`μ΄ λνλ μ  μλ€.

![](https://images.velog.io/images/abcd8637/post/fc214fc9-1d40-4f79-9752-354fefb6c492/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-02%2013.47.17.png)

---
## π Uncaught SyntaxError: Unexpected token '<β μ€λ₯ ν΄κ²°νκΈ°
μ¬μ΄νΈμ λ°°ν¬νκΈ° μν΄ `Webpack`μ μ¬μ©νμ¬ `npm run build` μ€ν ν `/dist`μ μλ `index.html`νμΌμ μ€νν΄λ΄€λλ° μ μμ μΌλ‘ νμ΄μ§κ° λνλμ§ μμ `console`μ°½μ μ΄μ΄λ³΄λ `Uncaught SyntaxError: Unexpected token '<β`μλ¬κ° λνλ¬λ€. ν΄κ²°λ°©λ²μ μν΄ κ΅¬κΈλ§μ νλ€ `GitHub`μ λμΌν <a href='https://github.com/webpack/webpack/issues/2882'>μ΄μ</a>κ° μμλ€. μμΈμΌλ‘λ `react-router`λ `js`νμΌμ΄ νμνμ§λ§, `html`νμΌμ μλ΅μΌλ‘ μ€¬κΈ°λλ¬Έμ μ€λ₯κ° μκΈ΄κ²μ΄μλ€. κ²°λ‘ μ μΌλ‘ `index.html`νμΌμ `<head></head>`νκ·Έ μ¬μ΄μ `<base href="/" />` μ½λλ₯Ό μΆκ°νλκΉ ν΄κ²°λμλ€.

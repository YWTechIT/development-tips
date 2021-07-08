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
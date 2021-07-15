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
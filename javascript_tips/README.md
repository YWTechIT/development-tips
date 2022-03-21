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
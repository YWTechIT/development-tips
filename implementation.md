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
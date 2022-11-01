## 📍 Schema type 중 리스트와 Non-null 타입에 대해 알아보자
`graphQL`로 `API` 요청을 할 때 쿼리에 다양한 타입을 넣을 수 있다. 스칼라 타입들이 기본으로 제공되는 것은 다들 알고 있을 것이다.
 
```javascript
/**
 * 1. Int: 부호가 있는 32비트 정수.
 * 2. Float: 부호가 있는 부동소수점 값.
 * 3. String: UTF-8 문자열
 * 4. Boolean: true | false
 * 5. ID: 캐시의 키로써 자주 사용되는 고유 식별자를 나타낸다. ID타입은 String과 같은 방법으로 직렬화되지만, ID로 정의하는 것은 사람이 읽을 수 있도록 하는 의도가 아닌것을 의미한다.
*/
```

하지만, 이번 작업에서 `API` 요청시 단순 스칼라 타입이 아니라 리스트 타입으로 요청하는 경우도 있다. 그럴 땐 어떻게 작성해야하는지, 그리고 `!`는 어떤 의미를 나타내는지 알아보자. 

리스트는 타입을 `List`로 표시하고 해당 타입의 배열로 반환한다. 리스트를 `type`으로 작성하는 것은 어렵지 않은데, 스칼라 값에 대괄호(`[]`)를 붙여주면 된다. 

다음은 `Non-Null`인데, `Non-Null`은 타입 뒤에 느낌표(`!`)를 추가하여 나타낸다. 그러면, 서버는 항상 이 필드에 대해 `null` 이 아닌 값을 반환하는 것을 예상하고 만약, `null`이 반환되면 오류를 반환한다.

마지막으로 `Non-Null`과 `List`를 결합하는 경우인데, 이게 은근히 헷갈린다. 첫번째로 `[]` 내부에 `!`를 가지는 경우다. (`$ids: [ID!]` ) 이 경우 `List` 자체는 `null` 일 수 있지만, `null`을 가질 수는 없다. 예를 들면 다음과 같다.

```javascript
// $ids: [ID!]
 
/**
 * ids: null // valid
 * ids: [] // valid
 * ids: ['123123', '234234'] // valid
 * ids: ['123123', null, '234234'] // error
*/
```

반대로 `!`를 리스트 바깥에 정의하면 목록 자체는 `null` 일 수 없지만, `null` 값을 포함할 수 있다.

```javascript
// $ids: [ID]!
 
/**
 * ids: null // error
 * ids: [] // valid
 * ids: ['123123', '234234'] // valid
 * ids: ['123123', null, '234234'] // valid
*/
```

그래서 `!`를 `List` 내부와 외부에 작성하면 잠재적인 에러를 방지할 수 있다. 

```javascript
// $ids: [ID!]!
 
/**
 * ids: null // error
 * ids: [] // valid
 * ids: ['123123', '234234'] // valid
 * ids: ['123123', null, '234234'] // error
*/
```

query는 다음과 같다.

```graphql
query GetPois($ids: [ID!]!){
 getPois(ids: $ids) {
   id
   type
   source
 } 
}

# query variables
{
  "ids": [
    "123-123-123",
    "234-234-234",
    "345-345-345",
  ]
}
```

Reference
- <a href ='https://graphql-kr.github.io/learn/schema/#non-null'>graphql-kr_Non-Null</a>

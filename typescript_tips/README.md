# typescript_tips

## 📍 핸들러 함수내부에서 중복으로 사용되는 함수 리팩토링하기
`React`로 컴포넌트가 아닌 컴포넌트 내부에서 `onClick`시 동작하는 `handler`함수를 만들다보면, `handler` 함수 내부에서 사용되는 함수를 조건으로 분기할 때 해당 함수가 중복으로 사용하는 경우가 종종 있다. 이번 포스팅은 애자일 소프트웨어 개발 방법의 하나인 `페어 프로그래밍(Pair programming)`으로 작업을 하다가 `네비게이터(navigator)`를 맡고 계신 헤일리가 발견해주신 케이스인데, 조건에 의해 인자 값이 바뀌어도 함수는 동일하게 사용할 때 중복되는 코드를 어떻게 하나로 합쳤는가에 대해 작성해보려 한다. 하단 코드는 리팩토링 전 `clickEventHandler` 함수의 코드이다. 이 코드로 컴파일하는데 에러는 없지만 리액트를 다뤄본 개발자라면 같은 역할을 하지만 중복된 코드를 봤다면 하나로 합치고 싶다는 생각이 뿜뿜 할 것이고 코드를 어떻게 개선했는지에 대해 작성해봤다. 리팩토링은 여러가지 방법이 있고, 개발자마다 그 방법이 다르기 때문에 이런 방법도 있구나라는 것에 초점을 맞춰주면 감사하겠다.(댓글에 피드백을 달아주시는것도 물론 환영이다.) 그럼 초기코드를 한번 살펴보자.(바쁘지 않다면 직접 리팩토링해보자.)

```typescript
// AS-IS
function clickEventHandler(
  onLinkClick?: LinkEventHandlerType,
  onImageClick?: ImageEventHandlerType,
): ImageEventHandlerType {
  return (event, image) => {
    if (image.link && image.link.hash && onLinkClick) {
      return onLinkClick(event, {
         href: `#${image.link.hash}`,
      })
    }
    if (image.link && image.link.href && onLinkClick) {
      return onLinkClick(e, {
          href: image.link.href,
        })
    } else if (onImageClick) {
      return onImageClick(event, image)
    }
  }
}
```

중복되는 코드를 찾았는가? 나는 7번과 13번의 `image.link && onLinkClick` 코드가 중복되는것을 찾았다. 더 좋은 방법도 있겠지만 `image.link && onLinkClick`를 하나의 조건문으로 합쳐보았다.

```typescript
function clickEventHandler(
  onLinkClick?: LinkEventHandlerType,
  onImageClick?: ImageEventHandlerType,
): ImageEventHandlerType {
  return (event, image) => {
    if (image.link && onLinkClick) {
      if (image.link.hash) {
        return onLinkClick(event, {
          href: `#${image.link.hash}`,
        })
      }

      if (image.link.href) {
        return onLinkClick(e, {
          href: image.link.href,
        })
      } else if (onImageClick) {
        return onImageClick(event, image)
      }
    }
  }
}
```

이렇게 하나의 조건문으로 합치고났는데, 다음엔 어떻게 리팩토링하면 좋을까?! 여러가지 방법이 있지만, 나는 `onLinkClick`의 중복사용을 줄이고 싶었다. 그래서 `onLinkClick` 함수를 한번만 사용하기위해 `image.link.hash`와 `image.link.href`값을 삼항연산자로 따로 빼내어 변수로 선언했다.

```typescript
function clickEventHandler(
  onLinkClick?: LinkEventHandlerType,
  onImageClick?: ImageEventHandlerType,
): ImageEventHandlerType {
  return (event, image) => {
    if (image.link && onLinkClick) {
      const href = image.link.hash ? `#${image.link.hash}` : image.link.href
      return onLinkClick(event, { href })
    } else if (onImageClick) {
      return onImageClick(event, image)
    }
  }
}
```

마지막으로 구조분해할당 문법을 사용하여 마무리했다. 8번 라인에서 `href: prevHref`로 작성한 이유는 9번 라인에서 `href` 변수로 선언해야 `onLinkClick` 함수의 두번째 인자를 `{ href: href }` 대신 `{ href }`로 사용 할 수 있기 때문이다.

```typescript
function clickEventHandler(
  onLinkClick?: LinkEventHandlerType,
  onImageClick?: ImageEventHandlerType,
): ImageEventHandlerType {
  return (event, image) => {
    if (image.link && onLinkClick) {
      const { hash, href: prevHref } = image.link
      const href = hash ? `#${hash}` : prevHref
      return onLinkClick(event, { href })
    } else if (onImageClick) {
      return onImageClick(event, image)
    }
  }
}
```

결론적으로 리팩토링 전과 후의 코드는 다음과 같다.

```typescript
// AS-IS
function clickEventHandler(
  onLinkClick?: LinkEventHandlerType,
  onImageClick?: ImageEventHandlerType,
): ImageEventHandlerType {
  return (event, image) => {
    if (image.link && onLinkClick) {
      if (image.link.hash) {
        return onLinkClick(event, {
          href: `#${image.link.hash}`,
        })
      }

      if (image.link.href) {
        return onLinkClick(e, {
          href: image.link.href,
        })
      } else if (onImageClick) {
        return onImageClick(event, image)
      }
    }
  }
}

// TO-BE
function clickEventHandler(
  onLinkClick?: LinkEventHandlerType,
  onImageClick?: ImageEventHandlerType,
): ImageEventHandlerType {
  return (event, image) => {
    if (image.link && onLinkClick) {
      const { hash, href: prevHref } = image.link
      const href = hash ? `#${hash}` : prevHref
      return onLinkClick(event, { href })
    } else if (onImageClick) {
      return onImageClick(event, image)
    }
  }
}
```

이렇게 중복사용되어 불필요하게 길어진 너저분한 코드를 리팩토링해봤다. 이번 리팩토링의 규모는 크진 않지만 작은 코드부터 리팩토링하다보면 언젠가 규모가 큰 코드도 금방 리팩토링 할 수 있지 않을까..?라는 생각을 하며 이번 포스팅을 마친다.

Reference
1. <a href='https://ko.wikipedia.org/wiki/%ED%8E%98%EC%96%B4_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D'>페어 프로그래밍 - 위키백과</a>
2. <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment'>구조 분해 할당 - MDN</a>


## 📍 query문의 value값에 key값과 동일한 값이 들어가있을 때
본인인증 기능을 구현하던 중 `query`에 `key=value`값이 하나씩 추가된 상태로 다른 페이지를 방문하다 마지막에 `query`문을 변수로 사용하려고 할 때 겪었던 일이다. 전체 query문은 `http://localhost:3001/foo/bar?returnUrl=returnUrl=/foo&id=baz`와 같았는데 `query`문을 분석하면 `returnUrl=returnUrl=/foo`가 한 묶음 `id=baz`가 한 묶음인 총 2개의 `query`가 나왔다. 두번째 `query`값은 별 이상이 없었으나, 첫번째 `query`가 이상했는데, 바로 첫번째 `query`의 `key`값인 `returnUrl`이 `value`값에도 들어가있다는 점이었다. 당시 `value`에 `returnUrl=`은 필요없기 때문에 해당 값을 제거해야 할 필요성을 느꼈는데, 나중에 매우 간편한 방법으로 해결했지만 처음엔 어떻게 제거해야할지 몰랐다. 그래서 <a href='https://github.com/ljharb/qs#stringifying'>qs 라이브러리의 encoder</a>을 이용했다. 사용방법은 `README`를 참고하자. `stringify`의 두번째인자에 `encoder` 함수를 선언하면 첫번째 인자에 `query`문이 넘어오게 되고, 네번째 인자인 `type`을 이용해 `type`이 `value`일 때 조건을 걸어 원하는 쿼리값이 넘어오면 `includes`를 통해 `split`을 하는방법을 사용했으나, 가독성이 매우 떨어지는 코드가 되었다. 그래서 곰곰이 생각 + 코드리뷰 끝에 떠올린것은 `query`문을 넘겨줄 때 `key`값을 만들어 넘기는 방법 대신 `qs.parse`를 이용하여 `key`값을 만들지 않고 `value`에 있는 값(`returnUrl=/foo`)의 `returnUrl`을 `key`값인 객체형태로 만들어 `qs.stringify`에 넘겨줄 때 `spread operator`를 사용했고 결과적으로 이전 코드보다 더 가독성이 좋아진 코드가 되었다. `qs.stringify`를 사용할 때 `key`값이 `value`에 중복선언 되어있어 제거해야하는 경우가 필요하다면 이 글처럼 `parse` + `...`로 해결해보자. 불필요하게 `encoder`를 사용하지 않고도 해결 할 수 있다.

```typescript
import { stringify } from 'qs'

// encoder example
var encoded = qs.stringify({ a: { b: 'c' } }, { encoder: function (str, defaultEncoder, charset, type) {
    if (type === 'key') {
        return // Encoded key
    } else if (type === 'value') {
        return // Encoded value
    }
}})

// before
const url = generateUrl({
    path,
    query: stringify(
      {
        returnUrl: returnUrlQuery,
        ordrIdxx,
      },
      {
        encoder: (query: string, _0, _1, type) => {
          if (type === 'value') {
            const value = 'returnUrl='
            const isValue = query.includes(value)
            if (isValue) {
              const [, extractValue] = query.split(value)
              return extractValue
            }
          }
          return query
        },
      },
    ),
  })

// after
import { stringify, parse } from 'qs'

const { path, query: returnUrlQuery } = parseUrl(returnUrl) // returnUrlQuery: returnUrl=/foo
const originalQuery = parse(returnUrlQuery)  // { returnUrl: '/foo' }

const url = generateUrl({
  path,
  query: stringify({
    ...originalQuery,
    baz,
  }),
  })  // url: /foo/bar?returnUrl=/foo
```

Reference
1. <a href='https://github.com/ljharb/qs#stringifying'>qs-stringify</a>

---
## 📍 이중 반복문에서 반복문 순회 후 타입 강제하기
언뜻 제목만 봐서는 이해하기 힘들 수 있지만, 쉽게 말해 타입이 2개이상인 `data`에서 `find`를 통해 나온 값에 원하는 `property`만 추출하고 싶을 때 사용하는 `assertion` 방법이다. 이미 `data`에 타입이 정해져 있는 경우라면 굳이 `type assertion` 해야되나?라고 생각할 수 있지만, `API`요청을 통해 받은 값(`data`)의 타입이 2개 혹은 2개 이상으로 설정되어있고, 내가 사용하고 싶은 `property`가 각각의 타입에 공통으로 들어있지 않은 `property`인데, 한쪽 타입의 `property`만 추출하면 컴파일 에러가 나는 경우 해결방법에 대해 글을 작성했다.

한가지 예시를 통해 살펴보자. `API`를 통해 받은 값 `data` 값이 있고, 그 `data`의 타입은 `data[][]`이다. 여기서 내가 원하는 로직은 `find`메소드를 사용해 `type===info`의 `value.text`값을 찾고 `type===info`의 `value.text`의 값이 없으면 `Unknown`으로 저장하여 결국엔 `string[]`을 반환하는 로직을 작성하고 싶다. 그런데 하단 코드블록의 `// Bad`처럼 작성하면 `ESlint`에서 `Unsafe assignment ~` 에러가 나오는 모습을 볼 수 있는데, 타입스크립트에서 `dataDocument`의 타입이 명확하지 않아 `any[]`인 타입으로 설정되어 나타나는 에러였다. 그래서 `as`를 사용해 `Not Good`처럼 작성했으나, 이번엔 `text` `property`를 찾지 못한다는 에러였다. `as`로 강제해줬는데 왜 저런 에러가 생기지..?라고 생각하며 30분의 삽질결과 얻은것은 `find` 메소드를 사용하고 나온 결과에 대해 타입을 `InfoType`으로 강제해줘야 `find` 메소드를 사용하고 나온 결과는 모두 `InfoType`이고, `InfoType` 내부에 있는 `value.text`에 접근 할 수 있음을 깨달았다. 이전단계에 `assertion`을 해야한다는 생각하지 못하고 최종단계인 `value.text` 값에 `assertion`하여 생긴 문제였었다! `Good`처럼 작성하면 자연스럽게 옵셔널체이닝(`?`)을 빼도 되므로 한층 가독성이 높아진 코드가 되었다. 나와 같은 문제로 고생하는 분들에게 도움이 되었으면 하는 바람으로 글을 마친다.

![](https://velog.velcdn.com/images/abcd8637/post/93a13de3-b2cc-42ae-8fec-3a28c654bd35/image.png)

![](https://velog.velcdn.com/images/abcd8637/post/f687c9af-c22b-47e8-b5a7-194f337c1d2d/image.png)

(22. 7. 5. )
`typescript`의 `사용자-정의 타입 가드(User-Defined Type Guards)`인 `is` 키워드를 사용 할 수도 있다. 여기서 타입가드란, 스코프 안에서의 타입을 보장하는 런타임 검사를 수행한다는 표현식이다. Use TypeGuard코드에서 `document is InfoType`코드를 통해 타입스크립트가 `document`의 타입의 범위를 `InfoType` 로 축소 시킬 수 있다. (피드백 주셔서 감사합니다. 우디데브님 :))

```typescript
// type.ts
interface InfoType {
  type: 'info'
  value: {
    text: string
  }
}

interface ImageType {
  type: 'images'
  value: {
    id: string
    source?: object
  }
}

type Data = InfoType | ImageType

// index.tsx
const data: Data[][] = [  
  [
    { type: "info", value: [Object] },
    { type: "images", value: [Object] },
  ],
  [
    { type: "info", value: [Object] },
    { type: "images", value: [Object] },
  ],
  [
    { type: "info", value: [Object] },
    { type: "images", value: [Object] },
  ],
  [
    { type: "info", value: [Object] },
    { type: "images", value: [Object] },
  ],
];

// Bad
const infoTextLabels: string[] = data.map(
  (dataDocument) =>
    dataDocument.find(({ type }) => type === 'info')?.value.text ||
    'Unknown',
)

// Not Bad
 const infoTextLabels: string[] = data.map(
    (dataDocument) =>
      (dataDocument.find(({ type }) => type === 'info')?.value
        .text as InfoType['value']['text']) || 'Unknown',
  )

// Good
const infoTextLabels: string[] = data
  .map(
    (dataDocument) =>
      (
        dataDocument.find(
          ({ type }) => type === 'info',
        ) as InfoType
      ).value.text || 'Unknown',
  )

// Use TypeGuard
const tabLabels = entries
  .map(
    (embeddedDocument) =>
      embeddedDocument.find(
        (document): document is InfoType =>
          document.type === 'heading3',
      )?.value.text || 'Unknown',
  )
```

Reference
1. https://typescript-kr.github.io/pages/advanced-types.html#%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%A0%95%EC%9D%98-%ED%83%80%EC%9E%85-%EA%B0%80%EB%93%9C-user-defined-type-guards
2. https://blog.logrocket.com/how-to-use-type-guards-typescript/#equality-narrowing-typeguard

---
## 📍 비구조할당문법에 type 선언하기
코드의 가독성을 높이기 위해 객체나 배열로 된 변수에 비구조화할당 문법(destructuring assignment)을 사용할 때가 자주 있다. `typescript`로 비구조화할당 문법을 사용하면 변수로 꺼낸 값에 타입을 정해주는 경우를 마주하는데 `object`형은 가끔 타입을 어떻게 정해야하는지 헷갈리곤 한다. 이번 글은 거창한 글보단 미세먼지처럼 작은 팁이지만 종종 헷갈릴 때 도움이 되므로 가벼운 마음으로 읽어보자.

하단 코드블록에 나와있듯이 `user`를 구조분해할당 이후 타입을 잘못 선언하는 경우는 `AS-IS`처럼 사용한다. 하지만, 저 문법은 타입을 정해주는 문법이 아니라, 객체의 원래 속성명을 다른 이름으로 할당하는 문법이다. 여기선 `string`으로 바꾼다는 의미다. 본래 의도라면 `TO-BE`처럼 선언해야 `firstname`과 `lastname`에 타입선언이 된다.

```typescript
const user = { firstname: 'ted', lastname: 'an' }
const { firstname, lastname } = user;

// AS-IS
const { firstname: string, lastname: string } = user;

// TO-BE
const { firstname, lastname }: { firstname: string; lastname: string } = user;
```

상단과 같은 경우는 굳이 `type casting`를 하지 않아도 컴파일러가 타입을 잘 읽어내는데, 문제는 보통 `api` 호출 이후 `res`값을 받을 때 컴파일러가 타입을 읽지 못하는 경우가 있다. 이럴 땐 `type assertion`을 종종 이용한다. 예를들어, 하단 코드블록 9번라인에 컴파일러가 `req.query`의 `type`이 명확하지 않다고 에러를 뿜는 경우인데, 이럴 땐, 응답하는 값이 강하게(`key`값 + `value` 타입을 아는 경우)정해져있는지, 약하게(`key`와 `value`의 타입만 아는경우) 정해져있는지 살펴본다.

이번 경우는 `query` 내부의 `property`의 `key`가 `returnUrl`으로 정해져있고, `value`가 `string`형으로 고정되어있다는 점을 알고 있어, `TO-BE 2`처럼 `type assertion`을 선언했다. 결론적으로 의도대로 컴파일러가 정상적으로 타입을 읽었다. 만약, 느슨하게 타입만 아는 경우라면 `TO-BE 1`, `TO-BE 1-1`처럼 작성 할 수도 있다는 점을 알아두자.

```typescript
const res = {
  query: {
    returnUrl: '/auth-web/my-accounts',
  },
  ...
}

// AS-IS
// ts2741: Property 'returnUrl' is missing in type '{ [key: string]: string | string[]; }' but required in type '{ returnUrl: string; }'.
const { returnUrl }: { returnUrl: string } = req.query

// TO-BE 1
const { returnUrl }: { [key: string]: string | string[] } = req.query

// TO-BE 1-1
interface ReqQueryType {
  [key: string]: string | string[]
}
const { returnUrl }: ReqQueryType = req.query

// TO-BE 2
const { returnUrl } = req.query as { returnUrl: string }
```

Reference
1. https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#%EC%83%88%EB%A1%9C%EC%9A%B4_%EB%B3%80%EC%88%98_%EC%9D%B4%EB%A6%84%EC%9C%BC%EB%A1%9C_%ED%95%A0%EB%8B%B9%ED%95%98%EA%B8%B0

---
## 📍 Formik으로 input state 쉽게 관리하기
<a href='https://ywtechit.tistory.com/475'>이전 글</a>에서 2개 이상의 `useState`를 하나로 묶어 `input`을 구현했었다. 그런데, `input`이 필요 할 때마다 `useState`로 생성하여 관리하고, 타입이 여러가지일 때 매번 그에 맞는 `handleChange`를 구현하는것은 상당히 번거롭다고 생각했다. 다른 개발자들도 이미 같은 생각을 했는지, 관련 라이브러리가 이미 존재하고 있었다. <a href='https://formik.org/docs/overview'>Formik</a>과 <a href='https://react-hook-form.com/'>React Hook Form</a> 라이브러리인데, 오늘은 `Formik`에 대해서 자세하게 알아보자. `Formik`은 `React`에서 `Form`을 구현할 때 가장 성가신 세 가지를 도와주는 라이브러리이다. 

```
1. 양식 상태 안팎에서 값 가져오기
2. 유효성 검사 및 오류 메시지
3. 양식 제출 처리
```

위의 모든 경우의 코드를 한곳에 배치함으로써 테스트와 리팩토링, 추론을 쉽게 할 수 있다고 공식문서에 나와있다. (By colocating all of the above in one place, Formik will keep things organized--making testing, refactoring, and reasoning about your forms a breeze.
) 결론적으로 내가 `Formik`을 사용하면서 가장 편하게 생각했던 점은 이전엔 `useState` 타입에 따라 `handleChange` 함수를 따로 생성해야하는 번거로움이 있었는데, `Formik`에서 제공하는 `handleChange`를 사용하면 함수를 따로 구현하지 않아도 해당 기능을 사용할 수 있다. 추가로 `handleBlur`, `handleSubmit`과 같은 함수도 자체적으로 제공하니 매번 함수를 작성하는 것보다 전체 코드가 줄어드는 상당한 이점이 있었다. 또한 `유효성(Validation)` 검사도 진행 할 수 있는데, `form` 형식을 제출 할 때 필수로 입력해야 하는 `input`이나, 구체적인 `minLength`, `maxLength` 글자 수를 정할 수도 있고, 정규 표현식으로 유효성 검사(`input` 값이 이메일 형식인지 아닌지)도 할 수 있다. 그리고 입력을 완료 한 후에 필드의 오류여부를 확인하게 도와주는 `errors` 기능, `form`을 만졌는지 확인하는 `touched`기능 등이 있다.

앞서 `Formik`에서는 `유효성(Validation)` 검사도 할 수 있다고 언급했는데, `Yup` 라이브러리를 통해 객체 스키마 유효성 검사를 진행 할 수 있다. <a href='https://formik.org/docs/tutorial#schema-validation-with-yup'>공식문서</a>에도 나와있듯이 유효성검사를 위해 `Yup`을 반드시 사용해야하는 것은 아니다(Yup is 100% optional.) 하지만, `Yup`을 사용하면 짧은 코드로도 정확하게 유효성 검사 기능을 사용할 수 있는 장점이있다. 짧은 코드는 가독성을 증가시켜주고 이는 협업시 생산성을 증가시키는 요소가 된다.

`Formik`의 기본적인 사용 방법은 <a href='https://formik.org/docs/overview#the-gist'>Getting Started</a>에도 나와 있지만, 내가 직접 마주했던 코드의 전/후 상태를 보면서 설명하는 것이 아무래도 기억에 오래 남을것 같아 코드를 일부 수정하여 가져왔다.

아래 코드블럭은 `Formik` 라이브러리를 사용하기 전의 코드이다. 아마 `React`를 사용하는 개발자들에겐 익숙한 코드이지 않을까 싶다. 코드를 대략적으로 설명하자면, `userEmail`과 `userName` 모두 `string` 타입이라 하나의 `useState`에 묶어서 선언했고, `personalInfo`, `adInfo` 모두 `boolean` 타입이라 마찬가지로 하나의 `useState`에 선언했다. 그리고 `handleInput`은 `string`이 변하는 값을 추적하는 함수이고, `handlePartTerms`는 `boolean` 타입이 변하는 값을 추적하는 함수, `handleTotalTerms`는 `boolean` 값을 한번에 모두 바꿔주는 함수이다. 크게보면 `handlePartTerms`와 `handleTotalTerms`의 성격이 같아 묶어서 선언할 수도 있지만 폼의 필드를 직접 수정하는 것과 더 위쪽 레이어에서 수정하는 것이 차이라는 관점에서 보면 나누는 편이 좋을 것 같아 핸들러를 따로 정의했다. 마지막으로 `handleSubmit`은 `form`에서 제출 할 때 실행되는 함수이다. 지금 작성한 코드보다 더욱 깔끔하게 작성 할 수 있는 방법은 무궁무진하지만, 현재 상태에서는 핸들러 코드가 많아 가독성이 떨어진다고 생각이든다.

```javascript
export default function WithoutFormik() {
  const [{ userEmail, userName }, setInput] = useState<InputType>({
    userEmail: '',
    userName: '',
  })
  const [{ personalInfo, adInfo }, setAgree] = useState<AgreeType>({
    personalInfo: false,
    adInfo: false,
  })

  const handleInput = (e: SyntheticEvent) => {
    const { id, value } = e.target as HTMLInputElement
    setInput((prevState) => ({ ...prevState, [id]: value }))
  }

  const handlePartTerms = (e: { target: HTMLInputElement }) => {
    const { id } = e.target

    setAgree((prevState) => ({
      ...prevState,
      [id]: !prevState[id],
    }))
  }

  const handleTotalTerms = (e: { target: HTMLInputElement }) => {
    const { checked } = e.target

    setAgree(
      checked
        ? {
            personalInfo: true,
            adInfo: true,
          }
        : {
            personalInfo: false,
            adInfo: false,
          },
    )
  }

  const handleSubmit = async () => {
    await applyServer({
      userEmail,
      userName,
      personalInfo,
      adInfo
    })
  }

  return(
    <>
      <Template />
    </>
  )
}
```

그렇다면, `Formik` 라이브러리를 사용한 코드는 어떻게 작성했는지 살펴보자. 하단 코드블럭을 보면 한눈에 봐도 코드가 많이 줄어든 것이 보이지 않는가? 또한, 군데군데 흩어져 있던 코드가 한곳에 모여 선언되어있으니 가독성도 많이 좋아졌다. 그리고 이전 코드에서는 유효성 검사 코드를 넣지 않았는데, 여기선 `validationSchema`이라는 유효성 검사 코드까지 같이 넣었다.(`Yup` 라이브러리로 사용했다.) `Formik`의 장점은 `handleChange` 핸들러를 타입별(`string`, `boolean`)로 따로 정의하지 않아도 `handleChange` 하나로 재사용 할 수 있다는 점과 유효성 검사를 진행할 때 `isValid`를 사용하여 유효성 검사가 모두 통과하지 않으면(이때는 `isValid`가 `false`가 된다.) `button`을 `disabled`로 만들 수 있다. 추가로 `input`의 최소 / 최대 길이까지 선언하여 해당 범위를 벗어나면 에러메시지도 띄울 수 있다. 그리고 `errors` 객체로 `validationSchema`의 유효성 검사가 통과하지 않았을 때 해당 메세지도 보여줄 수 있다. 마지막으로 `form`을 제출하고 응답이 오기전까지 `button`을 `disabled`로 만드는 `isSubmitting`까지 별도의 로직 선언없이 사용할 수 있는 점이 마음에 들었다.

```javascript
export default function WithFormik() {
  const {
    values,
    errors,
    isValid,
    isSubmitting,
    setValues,
    handleChange,
    handleSubmit,
  } = useFormik({
    initialValues: {
      userEmail: '',
      userName: '',
      personalInfo: false,
      adInfo: false,
    },
    validationSchema: Yup.object({
      userEmail: Yup.string()
        .email('이메일 형식이 아닙니다.')
        .required('이메일을 입력해 주세요.'),
      userName: Yup.string().required('닉네임을 입력해 주세요.'),
      personalInfo: Yup.bool().isTrue('개인정보 수집에 동의해주세요.'),
      adInfo: Yup.bool().isTrue('광고성 정보 수신에 동의해주세요.'),
    }),
    onSubmit: async ({email, nickname, personalInfo, adInfo}) => {
      await applyServer({
        email,
        nickname,
        personalInfo,
        adInfo,
      })
    },
  })

  const { userEmail, userName, personalInfo, adInfo } = values
  const isActive = isValid && !isSubmitting

  const allTermsHandleChange = () => {
    void setValues(
      checkedAllTerms
        ? {
            userEmail,
            userName,
            personalInfo: false,
            adInfo: false,
          }
        : {
            userEmail,
            userName,
            personalInfo: true,
            adInfo: true,
          },
    )
  }

  return(
    <>
      <Template />
    </>
  )
}
```

이밖에도 `React Context`와 동일하게 사용가능한 `useFormikContext`, `form`태그에 `handleSubmit`을 사용하지 않아도 되는 `<Form />`, `input` 태그에 `handleChange`를 사용하지 않아도 되는 `<Field />`태그 등 다양한 기능이 있다. 그러나, `Formik`에도 단점은 존재한다. 정해진 태그를 사용해야하는 점, 복잡한 `form`을 다루기가 어렵고 내가 원하는대로 커스텀하기가 일반적인 `form`에 비해 조금은 불편하다는 점. 그러나, 이런 단점을 극복할 수 있는 장점(`input` 로직을 일일이 구현하기 귀찮거나 협업할 때 등)이 더 많기 때문에 한번쯤 `Formik`을 사용하는 것을 권하며 이 글을 마친다.   

Reference
1. https://formik.org/docs/overview
2. https://github.com/jquense/yup
3. https://formik.org/docs/overview#the-gist
4. https://formik.org/docs/migrating-v2#isvalid
5. https://formik.org/docs/api/form
6. https://formik.org/docs/api/field
7. https://react-hook-form.com/

---
## 📍 2개 이상의 동일한 기능인 useState와 handleChange를 각각 하나로 관리하기
구독신청 페이지를 만들면서 개인정보수집 checkbox와 광고성 정보 수신 `input checkbox`를 `useState`로 관리하려고 하는데, 한 줄씩 `useState`로 선언하여 관리하니까 코드가 불필요하게 길어져 좋은 코드라고 보기 힘들었다. 그래서 기존에 `const [state, setState] = useState(false)`처럼 선언했다면 이를 `object`로 묶어 선언하니 코드가 줄어들었다. 

```typescript
import { useState } from "react";

// 리팩토링 전
export default function Subscription() {
  const [allAgree, setAllAgree] = useState<boolean>(false);
  const [privateAgree, setPrivateAgree] = useState<boolean>(false);
  const [adAgree, setAdAgree] = useState<boolean>(false);
}


// 리팩토링 후
interface AgreeType {
  personalInfo: boolean;
  adInfo: boolean;
}

export default function Subscription() {
  const [{ personalInfo, adInfo }, setAgree] = useState<AgreeType>({
    personalInfo: false,
    adInfo: false,
  });
}
```

또한, `checkbox`의 `state`를 변경하기 위해 `setAgree`의 값을 변경해야하는데, 리팩토링 전에는 `allAgree`만을 위한 `allHandleChange`를 만들었고, 나머지 `privateAgree`, `adAgree`를 위한 `handleChange` 함수를 만들었는데, 마찬가지로 변수명만 다를뿐 동일한 역할을 하는데도 함수를 2개 이상 관리해야하기 때문에 가독성이 매우 좋지 않았고, 함수 1개로 관리하기 위해 `input`의 `id`값으로 조건을 구분하여 `check`를 `toggle` 할 수 있게 설정했다. 그리고 `전체 동의합니다` 기능은 해당 `input`의 `checked` property에 `personalInfo`, `adInfo`가 참일 때만 `checked`가 될 수 있게 설정했다. 이때 가독성을 위해 모두 동의 했을 때의 조건을 `const checkedAllInfo = personalInfo && adInfo` 변수로 관리했다. 여담으로 66번 라인을 `else`로 처리 할 수 있지 않는가라고 생각할 수 있지만, 7번 라인에 `AgreeType`타입을 정의해놓았기 때문에 `else` 조건에 `personalInfo` 혹은 `adInfo` 외에 다른 `id`가 온다면 타입스크립트에서 제대로 처리 할 수 없다. 만약, `else`를 넣고 싶다면 7번라인에 `AgreeType`에 `[key in string]: boolean`을 넣어 다른 `id`가 오더라도 그 `id`가 `boolean` 타입이라고 정의하면 된다.

```typescript
import { useState } from "react";

// 리팩토링 전
export default function Subscription() {
  const [allAgree, setAllAgree] = useState<boolean>(false);
  const [privateAgree, setPrivateAgree] = useState<boolean>(false);
  const [adAgree, setAdAgree] = useState<boolean>(false);

  const allHandleChange = () => {
    if (allAgree) {
      setAllAgree(false);
      setPrivateAgree(false);
      setAdAgree(false);
    } else {
      setAllAgree(true);
      setPrivateAgree(true);
      setAdAgree(true);
    }
  };

  const handleChange = (
    setState: React.Dispatch<React.SetStateAction<boolean>>
  ) => {
    setState((state) => !state);
  };

  return (
    <>
      <Section centered margin={{ top: 60 }}>
        <label htmlFor="agree-all">
          <input
            id="agree-all"
            type="checkbox"
            checked={allAgree}
            onChange={allHandleChange}
          />
          전체 동의합니다.
        </label>

        <label htmlFor="agree-private">
          <input
            id="agree-private"
            type="checkbox"
            checked={privateAgree}
            onChange={() => handleChange(setPrivateAgree)}
          />
          개인정보 수집 및 이용에 동의합니다. (필수)
        </label>

        <label htmlFor="agree-advertise">
          <input
            id="agree-advertise"
            type="checkbox"
            checked={adAgree}
            onChange={() => handleChange(setAdAgree)}
          />
          광고성 정보 수신에 동의합니다. (필수)
        </label>

        <Button disabled={!(privateAgree && adAgree)}>구독하기</Button>
      </Section>
    </>
  );
}

// 리팩토링 후 
interface AgreeType {
  personalInfo: boolean;
  adInfo: boolean;
}

export default function Subscription() {
  const [{ personalInfo, adInfo }, setAgree] = useState<AgreeType>({
    personalInfo: false,
    adInfo: false,
  });
  const checkedAllInfo = personalInfo && adInfo;

  const handleChange = (e: { target: HTMLInputElement }) => {
    const { id, checked } = e.target;

    if (id === "all") {
      checked
        ? setAgree(() => ({
            personalInfo: true,
            adInfo: true,
          }))
        : setAgree(() => ({
            personalInfo: false,
            adInfo: false,
          }));
    } else if (id === "personalInfo" || id === "adInfo") {
      setAgree((state) => ({
        ...state,
        [id]: !state[id],
      }));
    }
  };

  return (
    <>
      <Section centered margin={{ top: 60 }}>
        <label htmlFor="all">
          <input
            id="all"
            type="checkbox"
            checked={personalInfo && adInfo}
            onChange={handleChange}
          />
          <Text size={17} bold>
            전체 동의합니다.
          </Text>
        </label>

        <label htmlFor="personalInfo">
          <input
            id="personalInfo"
            type="checkbox"
            checked={personalInfo}
            onChange={handleChange}
          />
          <Text size="medium">개인정보 수집 및 이용에 동의합니다. (필수)</Text>
        </label>

        <label htmlFor="adInfo">
          <input
            id="adInfo"
            type="checkbox"
            checked={adInfo}
            onChange={handleChange}
          />
          <Text size="medium">광고성 정보 수신에 동의합니다. (필수)</Text>
        </label>

        <Button disabled={!checkedAllInfo}>구독하기</Button>
      </Section>
    </>
  );
}
```

![](https://velog.velcdn.com/images/abcd8637/post/09e69e6f-01b7-49a8-bfb5-adb3cf3af32d/image.gif)

---
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

---
## 📍 useLayoutEffect, useEffect의 차이점 알아보기
`react`를 사용 할 때 `paint screen` 단계 이후 즉, 컴포넌트가 화면에 렌더링 된 이후 `사이드 이펙트(side Effect)` (여기서 말하는 사이드 이펙트는 컴포넌트 내에서 데이터 가져오기, 구독 설정하기, 수동으로 컴포넌트의 DOM을 조작하는 작업 등을 말한다.)를 비동기적으로 호출할 때 주로 `useEffect`를 사용한다. 

아래 사진은 <a href='https://github.com/donavon/hook-flow'>github</a>에서 가져온 `hook-flow` 사진인데, 한눈에 알 수 있어서 가져와봤다.

![](https://images.velog.io/images/abcd8637/post/2cb835c9-eb26-4d1b-bd14-711c8a1695d5/hook-flow.png)

다시 본론으로 돌아가서 화면에 `state`의 값이 바뀌어서 화면이 깜빡이거나 `DOM`을 호출하기 전에 무언가를 변경하고 싶을 때는 어떻게 해야할까? `paint screen` 단계 이전 즉, 화면이 그려지기 바로 전에 값을 동기적으로 호출하고 싶다는 생각이 들면 `useLayoutEffect` hook을 사용하면 된다.

정말로 `useLayoutEffect`는 화면에 그려지기 전에 실행되는지 궁금하여 간단한 실험을 진행했다. 만약, `localStorage`에 저장된 값을 불러오는 작업을 사이드이펙트로 관리한다고 했을 때, `useEffect`, `useLayoutEffect` 중 어떤 코드로 작성해야 적합할까?

잘모르겠다면 하단의 `gif`를 보고 어떤 `gif`가 `useEffect` 혹은 `useLayoutEffect`를 사용했을지 한번 고민해보자.

![](https://images.velog.io/images/abcd8637/post/2bba6e7a-a5e7-443b-8b84-349e5a58b8d4/useLayoutEffect.gif)

![](https://images.velog.io/images/abcd8637/post/35c4d223-5afc-4b52-91cb-ecd43f5cfbdf/useEffect.gif)

너무 쉬웠는가? 정답은 첫번째 `gif`가 바로 `useLayoutEffect`를 사용한 코드이고 두번째 `gif`는 `useEffect`를 사용한 코드이다. 어떤 차이점이 있는지 눈을 크게 뜨고 보면 보일텐데, 두번째 `gif`는 화면에 렌더링 된 이후에 `localStorage` 값을 가져오기 때문에 처음에는 빈 공간이었다가 나중에 값이 채워지는것을 알 수 있고 반면에, 첫번째 `gif`는 브라우저에 그려지기 전에 로직을 실행하므로 말끔하게 가져오는것을 알 수 있다.

이런 세세한 부분까지 신경써야해..?라는 생각이 들 수도 있다. 그러나 실험과 같이 값이 작은 경우에는 미미할 수 있지만 실제 배포 이후 값이 커졌을 때, 브라우저가 조금이라도 느려지게 된다면 고객의 경험성이 좋지 않을 수 있다. 남들과 경쟁력있는 `front-end`가 되려면 이런 디테일한 부분까지 캐치 할 수 있어야 한다고 생각한다.

reference
1. <a href='https://ko.reactjs.org/docs/hooks-effect.html'>react-docs-hooks-effect</a>

---
## 📍 object(key:value) type, interface 선언하기
`TS`를 사용하다가 `string`, `number`, `boolean`, `array`와 같은 자료형에는 `type`을 잘 선언할 수 있었는데, 유독 `key:value`와 같은 `object`는 `type`을 어떻게 선언해야하는지 헷갈렸다. 나는 이때까지 `key:value`형태의 `object`는 다음과 같이 선언했다.

```ts
interface PostType{
    sort: string;
}

const post: PostType = {sort: "asc"}
console.log(post);
👉🏽 { sort: 'asc' } 
```

`ReactQuery`를 사용하여 `useInfiniteQuery`를 구현하는 도중 함수 `parameter`로 반환되는 `key:value` 형태인 `post`값의 `key`는 `type`만 알고 `값`은 정확하지 않을 때 혹은 `post`값의 `value`는 `type`만 알고 `값`은 정확하지 않을 때, 혹은 `비구조할당문법(destructuring assignment)`을 사용했을 때는 `type`을 어떻게 선언해야하는지 궁금했다. 그래서 배워보자. how to declare object type!! 

```ts
// post의 `key`는 잘 모르지만 `type`은 확실 할 때
type SortType = {[key in string]: string};
const post = {sort: "asc"};
const {sort} = post as SortType;

// post의 `key` 값을 몇 개 알고 있을 때
type Keys = "sort" | "filter" 
type SortType = {[key in Keys]: string};
const post = {sort: "asc"};
const {sort} = post as SortType;

// post의 `value` 값을 몇 개 알고 있을 때
type Values = "asc" | "desc" | "id";
type SortType = {[key in string]: Values};
const post = {sort: "asc"};
const {sort} = post as SortType;

// post의 `key:value`를 대략적으로 알고 있을 때
type Keys = "sort" | "filter" 
type Values = "asc" | "desc" | "id";
type SortType = {[key in Keys]: Values};
const post = {sort: "asc"};
const {sort} = post as SortType;

// post의 정확한 `key:value` 모두 확실한 값일 때
type SortType = {sort: string};
const post = {sort: "asc"};
const {sort} = post as SortType;

// array형태로 return되는 post type 선언
type PostObj = {[key in string]: string};
type PostType = Array<string | PostObj>
const post: PostType = ["usePosts", {sort: "asc"}];
```

지금까지 작성한 코드를 토대로 살펴보면 `비구조할당`의 `type`선언은 변수위치가 아닌 할당하는 값 뒤에 `as`형태로 붙였다. 이를 한국말로 다운 캐스팅 혹은 영어로 `Type Assertions`이라고 부른다. 이와 관련한 <a href='https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions'>TypeScriptLang</a>에 나와있는 영어를 번역하면 다음과 같은 글을 읽을 수 있다. 때때로 `TS`가 알 수 없는 값의 유형에 대한 정보가 있을 것입니다. (...중략...) 이러한 상황에서 `Type Assertions`을 사용하여 보다 구체적인 유형을 지정할 수 있습니다. 또한, `TypeScript Deep Dive`에는 `TypeScript`에서는 시스템이 추론 및 분석한 타입 내용을 우리가 원하는 대로 얼마든지 바꿀 수 있습니다. 이때 "타입 표명(type assertion)"이라 불리는 메커니즘이 사용됩니다. TypeScript의 타입 표명은 프로그래머가 컴파일러에게 내가 너보다 타입에 더 잘 알고 있고, 나의 주장에 대해 의심하지 말라고 하는 것과 같습니다. 쉽게 말해 내가 타입을 선언하니까 내 말대로 해가 된다.

reference
1. <a href='https://www.designcise.com/web/tutorial/how-to-specify-types-for-destructured-object-properties-using-typescript'>How to Specify Types for Destructured Object Properties Using TypeScript?</a>
2. <a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions">typescriptlang</a>
3. <a href="https://radlohead.gitbook.io/typescript-deep-dive/type-system/type-assertion">TypeScript Deep Dive</a>
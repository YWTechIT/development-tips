# css-tips
### 📍 반응형을 고려하여 Cards 컴포넌트 중앙 정렬하기
CSS를 만지다보면 항상 내 뜻대로 안 될 때가 많다. ~~그럴 때마다 직접 손으로 움직여주고 싶은 마음이 굴뚝 같지만 프로그래머이니까 코드로 움직이는 방법을 사용하자.~~ 

여러가지 `Card`를 모아놓은 `Cards` 컴포넌트가 있다고 가정하자. 이 `Cards` 컴포넌트를 중앙 정렬하고 싶다고 했을 때 어떻게 할 것인가? 여러가지 방법을 떠올릴 수 있겠지만 처음에 나는 피그마에 명시되어있는 `margin`값을 고정하여 `Cards`를 중앙에 위치시켰다. 그런데 반응형이미지를 고려하는 환경에서 이러한 방법은 적절하지 못한 선택이었다. 왜냐하면 `viewport`가 바뀔때마다 `margin`값도 같이 변하는데, `margin`값을 하나로 고정해놨기 때문에 해당 `viewport`말고 다른 `viewport`에서 CSS가 원하는대로 나오지 않은 확률이 높기 때문이다. 이럴때는 `margin`값을 고정하지말고 전체 `Cards`의 `width`를 주고, `margin: ? auto`(?는 상+하 값이므로 필요한만큼 선언하자 중요한 값은 `auto`이다.)를 주면 반응형에 대응 할 수 있다. 

예를 들어, `viewport`가 `1280px`일 때 `Cards`의 `margin`은 `0 286px`을 줬다. 그랬을 때 첫번째 사진처럼 원하는 모양이 나오긴하지만, 브라우저 창의 크기가 커지게 되면 `margin`값은 고정되어있기 때문에 `Cards`의 전체적인 `Width`가  고정된 `margin`값까지 늘어나게 된다. 그렇게되면 두번째 사진처럼 `Card`가 늘어져보인다. 이럴 때 `Cards`의 `width`를 고정하고 `margin: 0 auto`를 주면 된다. 그러면 마지막 사진처럼 `1280px`보다 늘어나게 되도 `width`값은 고정되어 있고, 필요한 `margin`값은 자동으로 늘어나기 때문에 더 이상 늘어나지 않는다.

![](https://velog.velcdn.com/images/abcd8637/post/da024b96-3221-444a-84d5-c21a062538fc/image.png)

![](https://velog.velcdn.com/images/abcd8637/post/34cbde47-20d8-430c-b80d-2837def19b78/image.png)

![](https://velog.velcdn.com/images/abcd8637/post/02f23e6c-a92e-4c79-a622-2cde9cab61a0/image.png)

---
### 📍 styled-component에서 태그가 다르지만 css는 동일 할 때 css를 변수로 관리하기
`styled-component`로 태그를 꾸미던 중 태그는 다르지만 나머지 스타일은 동일하게 설정하는 경우가 있었다. 그냥 스타일을 중복해서 사용해도 되지만 그것보다 동일한 `css`를 변수로 관리하여 각각 적용하면 코드를 중복해서 사용하지 않는다.

```typescript
import styled from 'styled-components'

// before
const divWrapper = styled.div`
  font-size: 14px;
  background-color: red;
`

const SectionWrapper = styled.section`
  font-size: 14px;
  background-color: red;
`

// after
import styled, { css } from 'styled-components'

const cssVariable = css`
  font-size: 14px;
  background-color: red;
`

const divWrapper = styled.div`
  ${cssVariable}
`

const SectionWrapper = styled.section`
  ${cssVariable}
`
```


---
### 📍 pseudo selector를 사용하여 불필요한 styled-component 사용하지 않기
css 스타일링을 위해 `styled-component`를 사용하다 보면 제일 마지막 글자만 다른 색상으로 나타내야 하는데, 번거롭게 `styled-component`를 하나 더 만들어서 관리하는 경우가 있다. 그럴 때 여러가지 방법이 있지만 가상 선택자를 이용하면 코드를 줄이고, 직관적으로 스타일을 꾸밀 수 있다. 예를 들자면, `FormTitle`을 만드는데 필수로 입력 해야하는 `*`를 `FormTitle` 제일 마지막에 붙여야하는 경우가 있다. 처음에는 `FormTitle`과 `Required` 컴포넌트를 따로 만들어서 하단 코드블록처럼 관리했다. 코드블럭에 대해 간단하게 설명하자면 `FormTitle`의 `props`에는 `required`가 들어오고(이때 `required`의 타입은 `boolean`이다.), `required`가 `true`이면 내부 `span`태그의 `color`는 `red`로 설정하고 `required`가 `false`이면 `color`를 설정하지 않는 코드이다.

```typescript
// form.tsx
interface FormTitleProps {
  required: boolean
}

const FormTitle = styled.div<FormTitleProps>`
  ${({ required }) =>
    required &&
    css`
      span {
        color: red;
      }
    `}
`

function App() {
    return (
      <FormTitle required>
        이메일 주소 <Required />
      </FormTitle>
  )
}

// required.tsx
export function Required() {
  return <span>*</span>
}
```

이렇게 작성하다보니 불필요하게 새로 `required.tsx`파일을 만들어 관리해야하는 번거로움과 코드가 한번에 읽히지 않는다는 문제가 있었다. 이럴 때 `pseudo selector` 중 하나인 `::after`를 사용하면 코드를 조금 더 직관적으로 바꿀 수 있다. `::after`는 `HTML`코드 뒤에 `content` property로 내용을 삽입 할 수 있다. 이와 비슷한 가상 선택자로는 `::before`가 있는데, 이는 `HTML`코드 앞에 `content` property로 내용을 삽입 할 수 있다. 결론적으로 `::after`를 사용하여 다음처럼 리팩토링을 진행했다.

```typescript
// form.tsx
interface FormTitleProps {
  required: boolean
}

const FormTitle = styled.div<FormTitleProps>`
  ${({ required }) =>
    required &&
    css`
      ::after {
        content: ${required ? "'*'" : ''};
      }
    `}
`

function App() {
    return (
      <FormTitle required>
        이메일 주소
      </FormTitle>
  )
}
```

스타일을 하나 지정한다고 해서 무조건 스타일 컴포넌트로 새로운 컴포넌트를 만들기 보다 가상선택자와 같은 css 기본 속성을 이용하면 더 간단하게 구현 할 수 있다는 점을 배웠다.

Reference
1. https://www.w3schools.com/cssref/sel_after.asp
2. https://www.w3schools.com/cssref/sel_before.asp
3. https://www.w3schools.com/cssref/tryit.asp?filename=trycss_content
# css-tips

### 📍 line-height 단위 알아보기
css로 `line-height`를 작성하다보면 값의 단위가 각기 다른 모습을 볼 수 있다. 가령, `normal`을 사용하거나, `number` 타입을 사용하거나, `em`을 붙이는 경우가 있다. 나는 주로 `px`를 사용하지만 동료들은 다른 단위를 사용하는 경우가 있어 기초적인 내용이지만 지금 기억해두면 추후에 공식문서를 찾는 시간을 아낄 수 있지 않을까 하는 마음에 글로 남긴다. (내용은 MDN을 참고했다.)

`line-height`는 주로 text line 사이의 거리를 조절 할 때 사용한다. `line-height`의 단위는 4가지가 있다.

```javascript
/* 
 * 1. <number>: line-height: 2.3;
 * 2. <length>: line-height: 3em;
 * 3. <percentage>: line-height: 50%;
 * 4. normal: line-height: normal;
 */ 
```

먼저 `normal`은 `user-agent`에 의존한다. Firefox를 포함한 데스크탑 브라우저에서 default값으로 대략 `1.2`의 값을 사용한다. (요소의 `font-family`에 따라 다르다.)

두번째로 `<number>` 타입이다. `unit`은 붙지않고 숫자 형태만 사용하는데, 자신의 `font-size * number`를 하면 된다. 4가지 방법중 선호하는 방식이고, `inherit` 속성으로 인해 예기치 못한 결과를 피할 수 있다.

세번째로 `<length>`이다. box height를 계산하는데 사용되며, `em`은 예기치 못한 결과를 생산한다. (`em`단위는 부모 요소의 글꼴 크기를 의미하는데, `부모 font-size * em`로 설정된다(Relative to the font size of the element itself.))

마지막으로 `<percentage>`이다. 마찬가지로 예기치 못한 결과를 생산한다 . (`percentage`는 자신의 `font-size`를 기준으로 한다. (Relative to the font size of the element itself.)))

#### 💡 Prefer unitless numbers for line-height values
앞서 `em`과 `percentage`는 예기치 못한 결과를 생산한다고 했는데,  왜 그런지 알아보기 위해 예시를 가져왔다.(<a href='https://developer.mozilla.org/en-US/docs/Web/CSS/line-height#prefer_unitless_numbers_for_line-height_values'>MDN</a>에 있는 예시와 예제 코드를 추가했다.)

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="dyqoXYY" data-editable="true" data-user="YWTechIT" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/YWTechIT/pen/dyqoXYY">
  line-height</a> by an (<a href="https://codepen.io/YWTechIT">@YWTechIT</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

첫 번째 `div`의 `line-height`는 잘 나오는데, 두 번째 `div`의 `line-height`는 찌그러진 것을 알 수 있다. 그 이유는 첫 번째 `div`의 `line-height`를 계산하면 `h1`의 `font-size`인 `30px * 1.1 = 33.3`이 나오게 되지만, 두 번째 `div`의 `line-height`는 `h1`의 `font-size`가 아니라 부모(`.box`)의 `font-size`를 곱하기 때문에 `15px * 1.1 = 16.5`가 된다. 그렇기 때문에 line-height를 사용할 땐 단위가 없는 값을 선호하게 되는 것이다.

#### 💡 접근성 고려(Accessibility concerns)
주요 단락 내용에 `line-height`는 최소 `1.5`를 사용하세요. 이것은 난독증처럼 인지 문제가 있는 사람들 뿐만 아니라 저시력자들에게 도움이 될 것입니다. text를 크게 보기 위해 페이지를 확대한 경우, 단위가 없는 값을 사용하면 줄 높이가 비례적으로 조정됩니다.

Reference
1. <a href='https://developer.mozilla.org/en-US/docs/Web/CSS/line-height'>line-height - MDN</a>
2. <a href='https://developer.mozilla.org/ko/docs/Learn/CSS/Building_blocks/Values_and_units#%EC%88%AB%EC%9E%90_%EA%B8%B8%EC%9D%B4_%EB%B0%8F_%EB%B0%B1%EB%B6%84%EC%9C%A8'>Values_and_units - MDN</a>

### 📍 styled-components를 이용해 특정 element hover시 tooltip 보여주기
React에서 styled-component로 디자인 작업 중 제목 그대로 특정 element에 마우스 hover시 tooltip을 보여주는 스펙이 있었다. 이때까진 특정 element에 hover만 하면 끝이었는데, 추가로 tooltip까지 동작하게 하는것은 처음이었다. 

내키는대로 코드를 작성했는데, 원하는대로 작동하지 않았다. 공식문서를 찾아보니 component selector 패턴을 통해 스타일에 지정된 다른 구성 요소를 보간하여 자동으로 생성된 클래스 이름을 참조할 수 있는 강력한 패턴이라고 나와있었다. 즉, `styled`로 선언한 컴포넌트를 다른 클래스에서 호출 할 수 있다는 이야기인데, 이것만으로는 원인을 해결하지 못했다.

긴 삽질 끝에 원인을 찾았는데, 첫번째는 `styled` 순서를 잘못 선언했고, 두번째는 Tooltip 컴포넌트의 렌더링 위치가 target 컴포넌트보다 먼저 선언했기 때문이었다. 

결론적으로 다음과 코드를 작성했을 때 정상적으로 동작하는것을 확인 할 수 있다.

<p class="codepen" style="height: 500px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-height="300" data-theme-id="dark" data-default-tab="js" data-slug-hash="PoBxbBK" data-editable="true" data-user="YWTechIT" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/YWTechIT/pen/PoBxbBK">
  styled-component hover</a> by an (<a href="https://codepen.io/YWTechIT">@YWTechIT</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>


### Reference
1. <a href='https://styled-components.com/docs/advanced#referring-to-other-components'>Referring to other components</a>

### 📍 parents div에 의해 unclickable 하게 되는 children div를 clickable하게 만들기
제목이 어려울 수 있지만 `background: transparent`인 Parent Div 밑에 `clickable` 할 수 있는 Child div가 있을 때 클릭하는 방법을 작성하려고 한다. 

기존에는 부모 Div만 클릭 할 수 있어 자식 Div는 클릭 할 수 없었지만 css의 <a href='https://developer.mozilla.org/en-US/docs/Web/CSS/pointer-events'>pointer-events</a> 속성을 이용하여 해결 할 수 있었다. 글로 이해하기 힘들다면 하단의 사진을 보자.(그림은 발(🦶🏾)로 그렸다.)

![](https://res.cloudinary.com/ywtechit/image/upload/v1669196249/fgjgacryljjfd4vuynoo.jpg)

내가 해결한 방법은 부모 Div에 `pointer-events: none`을 주고, `clickable` 할 `target Div`에 `pointer-events: all`을 주자. 추가로 `pointer-events`는 모든 브라우저에 지원되는 속성이다. 

하단 `CodePen`에 당시 상황을 동일하게 구현하려했으나 `AS-IS`를 완벽하게 구현하지 못해 `unclickable`에 임의로 `pointer-events: none`을 줬다. 여기에 초점을 맞추는 대신 `clickable`에 `pointer-events: all`을 줌으로써 부모 Div에 의해 클릭이 불가능한 컴포넌트에도 `clickable` 할 수 것에 초점을 맞추면 된다. 

<iframe height="300" style="width: 100%;" scrolling="no" title="pointer-events" src="https://codepen.io/YWTechIT/embed/KKeoMRa?default-tab=js%2Cresult&editable=true&theme-id=dark" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/YWTechIT/pen/KKeoMRa">
  pointer-events</a> by an (<a href="https://codepen.io/YWTechIT">@YWTechIT</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

#### Reference
- <a href='https://developer.mozilla.org/en-US/docs/Web/CSS/pointer-events'>pointer-events - MDN</a>

### 📍 공백이 포함된 문장 모두 줄바꿈하기
반응형을 고려하며 공백이 포함된 문장까지 브라우저의 너비가 줄어들면 해당 문장을 모두 줄바꿈해야하는 니즈가 생겼다. 그래서 줄바꿈 관련 CSS인 <a href='https://developer.mozilla.org/ko/docs/Web/CSS/word-break'>word-break</a>와 <a href='https://developer.mozilla.org/ko/docs/Web/CSS/white-space'>white-space</a> 그리고 <a href='https://developer.mozilla.org/ko/docs/Web/CSS/overflow'>overflow</a> 등을 살펴보며 해답을 생각했지만 뾰족한 수가 나오지 않았다.

그래서 팀원들에게 현재 생긴 이슈를 공유하며 해결법을 찾고있는데, 공백을 뜻하는 `&nbsp;`와 `word-break: keep-all` 속성을 이용하면 해결할 수 있다는 팀원의 말을 듣고 작성해보니 해결됐다.

추가로 `&nbsp;`란, `Non-breaking Space`의 약어인데, 단어 뜻 그대로 공백을 의미한다. 특수기호가 붙은 이유는 일반적인 문자과 구분하기 위해서다. 외관상으로 공백이 있는것처럼 보이지만, 이 위치에서 줄바꿈이 될 수 없다는 뜻이다.

여담으로, `React Component`에서 `&nbsp;`를 작성하면 가끔 문자 그대로 출력되는 경우가 있는데, 이럴 때는 `HTML`코드인 `&nbsp;`를 넣는게 아니라, 유니코드 문자인 `\u00A0`을 넣으면 된다.

<iframe height="300" style="width: 100%;" scrolling="no" title="&amp;nbsp + keep-all" src="https://codepen.io/YWTechIT/embed/qBoKMJE?default-tab=html%2Cresult&editable=true&theme-id=dark" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/YWTechIT/pen/qBoKMJE">
  &amp;nbsp + keep-all</a> by an (<a href="https://codepen.io/YWTechIT">@YWTechIT</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

<iframe height="300" style="width: 100%;" scrolling="no" title="Untitled" src="https://codepen.io/YWTechIT/embed/NWYzLVM?default-tab=html%2Cresult&editable=true&theme-id=dark" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/YWTechIT/pen/NWYzLVM">
  Untitled</a> by an (<a href="https://codepen.io/YWTechIT">@YWTechIT</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

Reference
1. https://ko.wikipedia.org/wiki/%EC%A4%84_%EB%B0%94%EA%BF%88_%EC%97%86%EB%8A%94_%EA%B3%B5%EB%B0%B1
2. https://www.compart.com/en/unicode/U+00A0

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
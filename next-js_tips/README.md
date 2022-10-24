# next-js_tips

## 📍 next/router 사용 시 무한 렌더링 이슈 해결하기
`Next.js`로 특정 페이지에 접속하면 무한 새로고침 되는 이슈를 발견했다. 원인은 `next/router - Router.replace` 때문이었는데, 클라이언트 사이드에서 `Router.replace`를 사용한 이유는 필터에 조건을 넣고 검색 버튼을 클릭하면 해당 필터 값을 쿼리에 추가하여 다른 사람에게 링크를 공유할 때 필터를 두 번 조작하지 않도록 하기 위함이었다. 그래서 `Router.replace`와 `state`를 잃지 않고 `pathname`과 `query` 값을 업데이트 할 수 있는 옵션인 `shallow Routing: true`를 추가했으나 무한 렌더링(새로고침) 되는 이슈가 생겼다. 

왜 이런 현상이 발생하는지 알아보기 위해 구글링 해본 결과 결론적으로 `next/router`를 사용할 때 리액트 내부에서 사용하는 `ContextAPI`를 사용하게 되고, `router.*`를 실행하면 내부 상태 값을 바꾸기 위해 필연적으로 리액트의 리렌더링을 발생시키므로 이를 해결하기 위해 `next/router` 대신 리액트의 `state`를 건들지 않고 브라우저 `history`를 사용하는 `window.history API`를 사용하는것이 좋다. 그러면 리액트의 상태를 건들지 않기 때문에 리렌더링이 발생하지 않는다는 <a href='https://github.com/vercel/next.js/discussions/18072'>글</a>을 봤다. 그래서 `Router.replace` 대신 `window.history.replaceState`를 사용해보니 무한 렌더링 이슈가 해결되었다. 

`issue`를 `tracking` 하는데 꽤 오랜 시간이 걸렸는데, 이후에 나와 같은 문제를 겪는 분들에게 도움이 되었으면 좋겠는 마음에 글을 남긴다.

```typescript
const newPath = `/tour/tour-products?${stringify(query)}`;

// AS-IS
Router.replace(newPath, undefined, {
  shallow: true,
})

// TO-BE
history.replaceState({}, '', newPath)
```

Reference
1. <a href='https://nextjs.org/docs/routing/shallow-routing'>Shallow Routing</a>
2. <a href='https://github.com/vercel/next.js/discussions/18072'>Ability to push changes to url without re-rendering #18072</a>

---
## 📍 Rewrites와 Redirects 알아보기
로컬에서 `Next.js`로 작업 할 때 한 개 프로젝트가 아니라 두 개 프로젝트를 동시에 실행해야하는 경우가 종종 있다. 예를 들면 로그인을 담당하는 A프로젝트(port: 3001)에서 본인인증을 담당하는 B프로젝트(port: 3000)를 실행하는 경우가 그런경우다. 이럴 때 A에서 B프로젝트로 넘어가려면 `http://localhost:3000/verifications`를 작성하고 다시 B에서 A프로젝트로 넘어 올때는 `http://localhost:3001/auth-web`을 작성하고 AWS를 통해 배포 할 때는 `path`만 남겨두고, 로컬에서 테스트 할 때는 다시 호스트와 포트를 붙이고.. 이렇게 수동적이고 번거로운 작업을 지속하다간 다른 작업을 하지 못할 것 같았다. 이럴 때 `next.config.js`에서 관련 설정을 할 수 있었는데, 바로 `Rewrites`와 `Redirects`이다.

`Rewrites`와 `Redirects`의 공통적인 특징은 들어오는 요청(incoming request)경로를 다른 경로(destination path)로 매핑해준다. 결정적인 차이는 바로 사용자가 알아차릴 수 있느냐인데, `Rewrites`는 destination path에 URL 프록시와 마스킹으로 인해 사용자의 눈에 변함이 없는(hasn't changed)것처럼 보여지고, 반대로 `Redirects`는 새 페이지로 라우팅되고 URL 변경사항을 보여주기 때문에 사용자의 입장에서 눈에 변함이 있는 것처럼 보여진다. 

`Rewrites`와 `Redirects` 모두 `next.config.js`에서 설정 할 수 있으며, 예시 코드는 다음과 같다. `source`는 `incoming request path` 즉, 요청이 들어온 경로를 의미하고, `destination`은 라우팅하고 싶은 `path`를 의미한다. `Redirects`는 `Rewrites`와 다르게 `permanent` property가 있는데, `true`일 경우 308 상태코드를 보내주고, `false`일 경우에는 307 상태코드를 보내준다. 상태코드 308과 307의 차이는 `redirect forever & cache`, `redirect temporary & is not cached`의 차이인데, 전통적으로 `temporary redirect`인 경우에는 302를, `permanent redirect`인 경우 301을 사용했으나, 많은 브라우저들이 `original method`와 관계없이 `redirect`의 요청 방법을 `GET`으로 사용했다. 예를 들어, 브라우저가 `/v2/users` 위치에 있는 302코드와 함께 `POST /v1/users`요청을 하는 경우, `POST /v2/users` 요청을 예상할 수 있지만 그대신 `GET /v2/users`를 요청 할 수 있다. 그래서 `Next.js`는 `307`, `308` 상태코드로 요청을 명시적으로 보존하기 위해 사용한다.

```javascript
// next.config.js

// Rewrites
module.exports = {
  async rewrites() {
    return [
      {
        source: '/about',
        destination: '/',
      },
    ]
  },
}

// Redirects
module.exports = {
  async redirects() {
    return [
      {
        source: '/about',
        destination: '/',
        permanent: true,
      },
    ]
  },
}
```

작성했던 내용을 토대로 서론에서 언급했던 A에서 B프로젝트로 넘어갈 때의 `Rewrites`와 B에서 A프로젝트로 넘어갈 때 `Rewrites`를 작성하면 다음과 같다.

```javascript
// A(3001) -> B(3000)
const nextConfig = {
  async rewrites() {
    return [
      {
        source: '/verifications/:path*',
        destination: `http://localhost:${B_PORT}/verifications/:path*`,
        basePath: false,
      },
     ]
  }
}

// B(3000) -> A(3001)
const nextConfig = {
  async rewrites() {
    return [
      {
        source: '/auth-web/:path*',
        destination: `http://localhost:${A_PORT}/auth-web/:path*`,
        basePath: false,
      },
   ]
  }
}
```

Reference
1. <a href='https://nextjs.org/docs/api-reference/next.config.js/rewrites'>Next.js Docs Rewrites</a>
2. <a href='https://nextjs.org/docs/api-reference/next.config.js/redirects'>Next.js Docs Redirects</a>
# typescript_tips

## ๐ query๋ฌธ์ value๊ฐ์ key๊ฐ๊ณผ ๋์ผํ ๊ฐ์ด ๋ค์ด๊ฐ์์ ๋
๋ณธ์ธ์ธ์ฆ ๊ธฐ๋ฅ์ ๊ตฌํํ๋ ์ค `query`์ `key=value`๊ฐ์ด ํ๋์ฉ ์ถ๊ฐ๋ ์ํ๋ก ๋ค๋ฅธ ํ์ด์ง๋ฅผ ๋ฐฉ๋ฌธํ๋ค ๋ง์ง๋ง์ `query`๋ฌธ์ ๋ณ์๋ก ์ฌ์ฉํ๋ ค๊ณ  ํ  ๋ ๊ฒช์๋ ์ผ์ด๋ค. ์ ์ฒด query๋ฌธ์ `http://localhost:3001/foo/bar?returnUrl=returnUrl=/foo&id=baz`์ ๊ฐ์๋๋ฐ `query`๋ฌธ์ ๋ถ์ํ๋ฉด `returnUrl=returnUrl=/foo`๊ฐ ํ ๋ฌถ์ `id=baz`๊ฐ ํ ๋ฌถ์์ธ ์ด 2๊ฐ์ `query`๊ฐ ๋์๋ค. ๋๋ฒ์งธ `query`๊ฐ์ ๋ณ ์ด์์ด ์์์ผ๋, ์ฒซ๋ฒ์งธ `query`๊ฐ ์ด์ํ๋๋ฐ, ๋ฐ๋ก ์ฒซ๋ฒ์งธ `query`์ `key`๊ฐ์ธ `returnUrl`์ด `value`๊ฐ์๋ ๋ค์ด๊ฐ์๋ค๋ ์ ์ด์๋ค. ๋น์ `value`์ `returnUrl=`์ ํ์์๊ธฐ ๋๋ฌธ์ ํด๋น ๊ฐ์ ์ ๊ฑฐํด์ผ ํ  ํ์์ฑ์ ๋๊ผ๋๋ฐ, ๋์ค์ ๋งค์ฐ ๊ฐํธํ ๋ฐฉ๋ฒ์ผ๋ก ํด๊ฒฐํ์ง๋ง ์ฒ์์ ์ด๋ป๊ฒ ์ ๊ฑฐํด์ผํ ์ง ๋ชฐ๋๋ค. ๊ทธ๋์ <a href='https://github.com/ljharb/qs#stringifying'>qs ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ encoder</a>์ ์ด์ฉํ๋ค. ์ฌ์ฉ๋ฐฉ๋ฒ์ `README`๋ฅผ ์ฐธ๊ณ ํ์. `stringify`์ ๋๋ฒ์งธ์ธ์์ `encoder` ํจ์๋ฅผ ์ ์ธํ๋ฉด ์ฒซ๋ฒ์งธ ์ธ์์ `query`๋ฌธ์ด ๋์ด์ค๊ฒ ๋๊ณ , ๋ค๋ฒ์งธ ์ธ์์ธ `type`์ ์ด์ฉํด `type`์ด `value`์ผ ๋ ์กฐ๊ฑด์ ๊ฑธ์ด ์ํ๋ ์ฟผ๋ฆฌ๊ฐ์ด ๋์ด์ค๋ฉด `includes`๋ฅผ ํตํด `split`์ ํ๋๋ฐฉ๋ฒ์ ์ฌ์ฉํ์ผ๋, ๊ฐ๋์ฑ์ด ๋งค์ฐ ๋จ์ด์ง๋ ์ฝ๋๊ฐ ๋์๋ค. ๊ทธ๋์ ๊ณฐ๊ณฐ์ด ์๊ฐ + ์ฝ๋๋ฆฌ๋ทฐ ๋์ ๋ ์ฌ๋ฆฐ๊ฒ์ `query`๋ฌธ์ ๋๊ฒจ์ค ๋ `key`๊ฐ์ ๋ง๋ค์ด ๋๊ธฐ๋ ๋ฐฉ๋ฒ ๋์  `qs.parse`๋ฅผ ์ด์ฉํ์ฌ `key`๊ฐ์ ๋ง๋ค์ง ์๊ณ  `value`์ ์๋ ๊ฐ(`returnUrl=/foo`)์ `returnUrl`์ `key`๊ฐ์ธ ๊ฐ์ฒดํํ๋ก ๋ง๋ค์ด `qs.stringify`์ ๋๊ฒจ์ค ๋ `spread operator`๋ฅผ ์ฌ์ฉํ๊ณ  ๊ฒฐ๊ณผ์ ์ผ๋ก ์ด์  ์ฝ๋๋ณด๋ค ๋ ๊ฐ๋์ฑ์ด ์ข์์ง ์ฝ๋๊ฐ ๋์๋ค. `qs.stringify`๋ฅผ ์ฌ์ฉํ  ๋ `key`๊ฐ์ด `value`์ ์ค๋ณต์ ์ธ ๋์ด์์ด ์ ๊ฑฐํด์ผํ๋ ๊ฒฝ์ฐ๊ฐ ํ์ํ๋ค๋ฉด ์ด ๊ธ์ฒ๋ผ `parse` + `...`๋ก ํด๊ฒฐํด๋ณด์. ๋ถํ์ํ๊ฒ `encoder`๋ฅผ ์ฌ์ฉํ์ง ์๊ณ ๋ ํด๊ฒฐ ํ  ์ ์๋ค.

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
## ๐ ์ด์ค ๋ฐ๋ณต๋ฌธ์์ ๋ฐ๋ณต๋ฌธ ์ํ ํ ํ์ ๊ฐ์ ํ๊ธฐ
์ธ๋ป ์ ๋ชฉ๋ง ๋ด์๋ ์ดํดํ๊ธฐ ํ๋ค ์ ์์ง๋ง, ์ฝ๊ฒ ๋งํด ํ์์ด 2๊ฐ์ด์์ธ `data`์์ `find`๋ฅผ ํตํด ๋์จ ๊ฐ์ ์ํ๋ `property`๋ง ์ถ์ถํ๊ณ  ์ถ์ ๋ ์ฌ์ฉํ๋ `assertion` ๋ฐฉ๋ฒ์ด๋ค. ์ด๋ฏธ `data`์ ํ์์ด ์ ํด์ ธ ์๋ ๊ฒฝ์ฐ๋ผ๋ฉด ๊ตณ์ด `type assertion` ํด์ผ๋๋?๋ผ๊ณ  ์๊ฐํ  ์ ์์ง๋ง, `API`์์ฒญ์ ํตํด ๋ฐ์ ๊ฐ(`data`)์ ํ์์ด 2๊ฐ ํน์ 2๊ฐ ์ด์์ผ๋ก ์ค์ ๋์ด์๊ณ , ๋ด๊ฐ ์ฌ์ฉํ๊ณ  ์ถ์ `property`๊ฐ ๊ฐ๊ฐ์ ํ์์ ๊ณตํต์ผ๋ก ๋ค์ด์์ง ์์ `property`์ธ๋ฐ, ํ์ชฝ ํ์์ `property`๋ง ์ถ์ถํ๋ฉด ์ปดํ์ผ ์๋ฌ๊ฐ ๋๋ ๊ฒฝ์ฐ ํด๊ฒฐ๋ฐฉ๋ฒ์ ๋ํด ๊ธ์ ์์ฑํ๋ค.

ํ๊ฐ์ง ์์๋ฅผ ํตํด ์ดํด๋ณด์. `API`๋ฅผ ํตํด ๋ฐ์ ๊ฐ `data` ๊ฐ์ด ์๊ณ , ๊ทธ `data`์ ํ์์ `data[][]`์ด๋ค. ์ฌ๊ธฐ์ ๋ด๊ฐ ์ํ๋ ๋ก์ง์ `find`๋ฉ์๋๋ฅผ ์ฌ์ฉํด `type===info`์ `value.text`๊ฐ์ ์ฐพ๊ณ  `type===info`์ `value.text`์ ๊ฐ์ด ์์ผ๋ฉด `Unknown`์ผ๋ก ์ ์ฅํ์ฌ ๊ฒฐ๊ตญ์ `string[]`์ ๋ฐํํ๋ ๋ก์ง์ ์์ฑํ๊ณ  ์ถ๋ค. ๊ทธ๋ฐ๋ฐ ํ๋จ ์ฝ๋๋ธ๋ก์ `// Bad`์ฒ๋ผ ์์ฑํ๋ฉด `ESlint`์์ `Unsafe assignment ~` ์๋ฌ๊ฐ ๋์ค๋ ๋ชจ์ต์ ๋ณผ ์ ์๋๋ฐ, ํ์์คํฌ๋ฆฝํธ์์ `dataDocument`์ ํ์์ด ๋ชํํ์ง ์์ `any[]`์ธ ํ์์ผ๋ก ์ค์ ๋์ด ๋ํ๋๋ ์๋ฌ์๋ค. ๊ทธ๋์ `as`๋ฅผ ์ฌ์ฉํด `Not Good`์ฒ๋ผ ์์ฑํ์ผ๋, ์ด๋ฒ์ `text` `property`๋ฅผ ์ฐพ์ง ๋ชปํ๋ค๋ ์๋ฌ์๋ค. `as`๋ก ๊ฐ์ ํด์คฌ๋๋ฐ ์ ์ ๋ฐ ์๋ฌ๊ฐ ์๊ธฐ์ง..?๋ผ๊ณ  ์๊ฐํ๋ฉฐ 30๋ถ์ ์ฝ์ง๊ฒฐ๊ณผ ์ป์๊ฒ์ `find` ๋ฉ์๋๋ฅผ ์ฌ์ฉํ๊ณ  ๋์จ ๊ฒฐ๊ณผ์ ๋ํด ํ์์ `InfoType`์ผ๋ก ๊ฐ์ ํด์ค์ผ `find` ๋ฉ์๋๋ฅผ ์ฌ์ฉํ๊ณ  ๋์จ ๊ฒฐ๊ณผ๋ ๋ชจ๋ `InfoType`์ด๊ณ , `InfoType` ๋ด๋ถ์ ์๋ `value.text`์ ์ ๊ทผ ํ  ์ ์์์ ๊นจ๋ฌ์๋ค. ์ด์ ๋จ๊ณ์ `assertion`์ ํด์ผํ๋ค๋ ์๊ฐํ์ง ๋ชปํ๊ณ  ์ต์ข๋จ๊ณ์ธ `value.text` ๊ฐ์ `assertion`ํ์ฌ ์๊ธด ๋ฌธ์ ์์๋ค! `Good`์ฒ๋ผ ์์ฑํ๋ฉด ์์ฐ์ค๋ฝ๊ฒ ์ต์๋์ฒด์ด๋(`?`)์ ๋นผ๋ ๋๋ฏ๋ก ํ์ธต ๊ฐ๋์ฑ์ด ๋์์ง ์ฝ๋๊ฐ ๋์๋ค. ๋์ ๊ฐ์ ๋ฌธ์ ๋ก ๊ณ ์ํ๋ ๋ถ๋ค์๊ฒ ๋์์ด ๋์์ผ๋ฉด ํ๋ ๋ฐ๋์ผ๋ก ๊ธ์ ๋ง์น๋ค.

![](https://velog.velcdn.com/images/abcd8637/post/93a13de3-b2cc-42ae-8fec-3a28c654bd35/image.png)

![](https://velog.velcdn.com/images/abcd8637/post/f687c9af-c22b-47e8-b5a7-194f337c1d2d/image.png)

(22. 7. 5. )
`typescript`์ `์ฌ์ฉ์-์ ์ ํ์ ๊ฐ๋(User-Defined Type Guards)`์ธ `is` ํค์๋๋ฅผ ์ฌ์ฉ ํ  ์๋ ์๋ค. ์ฌ๊ธฐ์ ํ์๊ฐ๋๋, ์ค์ฝํ ์์์์ ํ์์ ๋ณด์ฅํ๋ ๋ฐํ์ ๊ฒ์ฌ๋ฅผ ์ํํ๋ค๋ ํํ์์ด๋ค. Use TypeGuard์ฝ๋์์ `document is InfoType`์ฝ๋๋ฅผ ํตํด ํ์์คํฌ๋ฆฝํธ๊ฐ `document`์ ํ์์ ๋ฒ์๋ฅผ `InfoType` ๋ก ์ถ์ ์ํฌ ์ ์๋ค. (ํผ๋๋ฐฑ ์ฃผ์์ ๊ฐ์ฌํฉ๋๋ค. ์ฐ๋๋ฐ๋ธ๋ :))

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
## ๐ ๋น๊ตฌ์กฐํ ๋น๋ฌธ๋ฒ์ type ์ ์ธํ๊ธฐ
์ฝ๋์ ๊ฐ๋์ฑ์ ๋์ด๊ธฐ ์ํด ๊ฐ์ฒด๋ ๋ฐฐ์ด๋ก ๋ ๋ณ์์ ๋น๊ตฌ์กฐํํ ๋น ๋ฌธ๋ฒ(destructuring assignment)์ ์ฌ์ฉํ  ๋๊ฐ ์์ฃผ ์๋ค. `typescript`๋ก ๋น๊ตฌ์กฐํํ ๋น ๋ฌธ๋ฒ์ ์ฌ์ฉํ๋ฉด ๋ณ์๋ก ๊บผ๋ธ ๊ฐ์ ํ์์ ์ ํด์ฃผ๋ ๊ฒฝ์ฐ๋ฅผ ๋ง์ฃผํ๋๋ฐ `object`ํ์ ๊ฐ๋ ํ์์ ์ด๋ป๊ฒ ์ ํด์ผํ๋์ง ํท๊ฐ๋ฆฌ๊ณค ํ๋ค. ์ด๋ฒ ๊ธ์ ๊ฑฐ์ฐฝํ ๊ธ๋ณด๋จ ๋ฏธ์ธ๋จผ์ง์ฒ๋ผ ์์ ํ์ด์ง๋ง ์ข์ข ํท๊ฐ๋ฆด ๋ ๋์์ด ๋๋ฏ๋ก ๊ฐ๋ฒผ์ด ๋ง์์ผ๋ก ์ฝ์ด๋ณด์.

ํ๋จ ์ฝ๋๋ธ๋ก์ ๋์์๋ฏ์ด `user`๋ฅผ ๊ตฌ์กฐ๋ถํดํ ๋น ์ดํ ํ์์ ์๋ชป ์ ์ธํ๋ ๊ฒฝ์ฐ๋ `AS-IS`์ฒ๋ผ ์ฌ์ฉํ๋ค. ํ์ง๋ง, ์  ๋ฌธ๋ฒ์ ํ์์ ์ ํด์ฃผ๋ ๋ฌธ๋ฒ์ด ์๋๋ผ, ๊ฐ์ฒด์ ์๋ ์์ฑ๋ช์ ๋ค๋ฅธ ์ด๋ฆ์ผ๋ก ํ ๋นํ๋ ๋ฌธ๋ฒ์ด๋ค. ์ฌ๊ธฐ์  `string`์ผ๋ก ๋ฐ๊พผ๋ค๋ ์๋ฏธ๋ค. ๋ณธ๋ ์๋๋ผ๋ฉด `TO-BE`์ฒ๋ผ ์ ์ธํด์ผ `firstname`๊ณผ `lastname`์ ํ์์ ์ธ์ด ๋๋ค.

```typescript
const user = { firstname: 'ted', lastname: 'an' }
const { firstname, lastname } = user;

// AS-IS
const { firstname: string, lastname: string } = user;

// TO-BE
const { firstname, lastname }: { firstname: string; lastname: string } = user;
```

์๋จ๊ณผ ๊ฐ์ ๊ฒฝ์ฐ๋ ๊ตณ์ด `type casting`๋ฅผ ํ์ง ์์๋ ์ปดํ์ผ๋ฌ๊ฐ ํ์์ ์ ์ฝ์ด๋ด๋๋ฐ, ๋ฌธ์ ๋ ๋ณดํต `api` ํธ์ถ ์ดํ `res`๊ฐ์ ๋ฐ์ ๋ ์ปดํ์ผ๋ฌ๊ฐ ํ์์ ์ฝ์ง ๋ชปํ๋ ๊ฒฝ์ฐ๊ฐ ์๋ค. ์ด๋ด ๋ `type assertion`์ ์ข์ข ์ด์ฉํ๋ค. ์๋ฅผ๋ค์ด, ํ๋จ ์ฝ๋๋ธ๋ก 9๋ฒ๋ผ์ธ์ ์ปดํ์ผ๋ฌ๊ฐ `req.query`์ `type`์ด ๋ชํํ์ง ์๋ค๊ณ  ์๋ฌ๋ฅผ ๋ฟ๋ ๊ฒฝ์ฐ์ธ๋ฐ, ์ด๋ด ๋, ์๋ตํ๋ ๊ฐ์ด ๊ฐํ๊ฒ(`key`๊ฐ + `value` ํ์์ ์๋ ๊ฒฝ์ฐ)์ ํด์ ธ์๋์ง, ์ฝํ๊ฒ(`key`์ `value`์ ํ์๋ง ์๋๊ฒฝ์ฐ) ์ ํด์ ธ์๋์ง ์ดํด๋ณธ๋ค.

์ด๋ฒ ๊ฒฝ์ฐ๋ `query` ๋ด๋ถ์ `property`์ `key`๊ฐ `returnUrl`์ผ๋ก ์ ํด์ ธ์๊ณ , `value`๊ฐ `string`ํ์ผ๋ก ๊ณ ์ ๋์ด์๋ค๋ ์ ์ ์๊ณ  ์์ด, `TO-BE 2`์ฒ๋ผ `type assertion`์ ์ ์ธํ๋ค. ๊ฒฐ๋ก ์ ์ผ๋ก ์๋๋๋ก ์ปดํ์ผ๋ฌ๊ฐ ์ ์์ ์ผ๋ก ํ์์ ์ฝ์๋ค. ๋ง์ฝ, ๋์จํ๊ฒ ํ์๋ง ์๋ ๊ฒฝ์ฐ๋ผ๋ฉด `TO-BE 1`, `TO-BE 1-1`์ฒ๋ผ ์์ฑ ํ  ์๋ ์๋ค๋ ์ ์ ์์๋์.

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
## ๐ Formik์ผ๋ก input state ์ฝ๊ฒ ๊ด๋ฆฌํ๊ธฐ
<a href='https://ywtechit.tistory.com/475'>์ด์  ๊ธ</a>์์ 2๊ฐ ์ด์์ `useState`๋ฅผ ํ๋๋ก ๋ฌถ์ด `input`์ ๊ตฌํํ์๋ค. ๊ทธ๋ฐ๋ฐ, `input`์ด ํ์ ํ  ๋๋ง๋ค `useState`๋ก ์์ฑํ์ฌ ๊ด๋ฆฌํ๊ณ , ํ์์ด ์ฌ๋ฌ๊ฐ์ง์ผ ๋ ๋งค๋ฒ ๊ทธ์ ๋ง๋ `handleChange`๋ฅผ ๊ตฌํํ๋๊ฒ์ ์๋นํ ๋ฒ๊ฑฐ๋กญ๋ค๊ณ  ์๊ฐํ๋ค. ๋ค๋ฅธ ๊ฐ๋ฐ์๋ค๋ ์ด๋ฏธ ๊ฐ์ ์๊ฐ์ ํ๋์ง, ๊ด๋ จ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๊ฐ ์ด๋ฏธ ์กด์ฌํ๊ณ  ์์๋ค. <a href='https://formik.org/docs/overview'>Formik</a>๊ณผ <a href='https://react-hook-form.com/'>React Hook Form</a> ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ธ๋ฐ, ์ค๋์ `Formik`์ ๋ํด์ ์์ธํ๊ฒ ์์๋ณด์. `Formik`์ `React`์์ `Form`์ ๊ตฌํํ  ๋ ๊ฐ์ฅ ์ฑ๊ฐ์  ์ธ ๊ฐ์ง๋ฅผ ๋์์ฃผ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ด๋ค. 

```
1. ์์ ์ํ ์ํ์์ ๊ฐ ๊ฐ์ ธ์ค๊ธฐ
2. ์ ํจ์ฑ ๊ฒ์ฌ ๋ฐ ์ค๋ฅ ๋ฉ์์ง
3. ์์ ์ ์ถ ์ฒ๋ฆฌ
```

์์ ๋ชจ๋  ๊ฒฝ์ฐ์ ์ฝ๋๋ฅผ ํ๊ณณ์ ๋ฐฐ์นํจ์ผ๋ก์จ ํ์คํธ์ ๋ฆฌํฉํ ๋ง, ์ถ๋ก ์ ์ฝ๊ฒ ํ  ์ ์๋ค๊ณ  ๊ณต์๋ฌธ์์ ๋์์๋ค. (By colocating all of the above in one place, Formik will keep things organized--making testing, refactoring, and reasoning about your forms a breeze.
) ๊ฒฐ๋ก ์ ์ผ๋ก ๋ด๊ฐ `Formik`์ ์ฌ์ฉํ๋ฉด์ ๊ฐ์ฅ ํธํ๊ฒ ์๊ฐํ๋ ์ ์ ์ด์ ์ `useState` ํ์์ ๋ฐ๋ผ `handleChange` ํจ์๋ฅผ ๋ฐ๋ก ์์ฑํด์ผํ๋ ๋ฒ๊ฑฐ๋ก์์ด ์์๋๋ฐ, `Formik`์์ ์ ๊ณตํ๋ `handleChange`๋ฅผ ์ฌ์ฉํ๋ฉด ํจ์๋ฅผ ๋ฐ๋ก ๊ตฌํํ์ง ์์๋ ํด๋น ๊ธฐ๋ฅ์ ์ฌ์ฉํ  ์ ์๋ค. ์ถ๊ฐ๋ก `handleBlur`, `handleSubmit`๊ณผ ๊ฐ์ ํจ์๋ ์์ฒด์ ์ผ๋ก ์ ๊ณตํ๋ ๋งค๋ฒ ํจ์๋ฅผ ์์ฑํ๋ ๊ฒ๋ณด๋ค ์ ์ฒด ์ฝ๋๊ฐ ์ค์ด๋๋ ์๋นํ ์ด์ ์ด ์์๋ค. ๋ํ `์ ํจ์ฑ(Validation)` ๊ฒ์ฌ๋ ์งํ ํ  ์ ์๋๋ฐ, `form` ํ์์ ์ ์ถ ํ  ๋ ํ์๋ก ์๋ ฅํด์ผ ํ๋ `input`์ด๋, ๊ตฌ์ฒด์ ์ธ `minLength`, `maxLength` ๊ธ์ ์๋ฅผ ์ ํ  ์๋ ์๊ณ , ์ ๊ท ํํ์์ผ๋ก ์ ํจ์ฑ ๊ฒ์ฌ(`input` ๊ฐ์ด ์ด๋ฉ์ผ ํ์์ธ์ง ์๋์ง)๋ ํ  ์ ์๋ค. ๊ทธ๋ฆฌ๊ณ  ์๋ ฅ์ ์๋ฃ ํ ํ์ ํ๋์ ์ค๋ฅ์ฌ๋ถ๋ฅผ ํ์ธํ๊ฒ ๋์์ฃผ๋ `errors` ๊ธฐ๋ฅ, `form`์ ๋ง์ก๋์ง ํ์ธํ๋ `touched`๊ธฐ๋ฅ ๋ฑ์ด ์๋ค.

์์ `Formik`์์๋ `์ ํจ์ฑ(Validation)` ๊ฒ์ฌ๋ ํ  ์ ์๋ค๊ณ  ์ธ๊ธํ๋๋ฐ, `Yup` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ํตํด ๊ฐ์ฒด ์คํค๋ง ์ ํจ์ฑ ๊ฒ์ฌ๋ฅผ ์งํ ํ  ์ ์๋ค. <a href='https://formik.org/docs/tutorial#schema-validation-with-yup'>๊ณต์๋ฌธ์</a>์๋ ๋์์๋ฏ์ด ์ ํจ์ฑ๊ฒ์ฌ๋ฅผ ์ํด `Yup`์ ๋ฐ๋์ ์ฌ์ฉํด์ผํ๋ ๊ฒ์ ์๋๋ค(Yup is 100% optional.) ํ์ง๋ง, `Yup`์ ์ฌ์ฉํ๋ฉด ์งง์ ์ฝ๋๋ก๋ ์ ํํ๊ฒ ์ ํจ์ฑ ๊ฒ์ฌ ๊ธฐ๋ฅ์ ์ฌ์ฉํ  ์ ์๋ ์ฅ์ ์ด์๋ค. ์งง์ ์ฝ๋๋ ๊ฐ๋์ฑ์ ์ฆ๊ฐ์์ผ์ฃผ๊ณ  ์ด๋ ํ์์ ์์ฐ์ฑ์ ์ฆ๊ฐ์ํค๋ ์์๊ฐ ๋๋ค.

`Formik`์ ๊ธฐ๋ณธ์ ์ธ ์ฌ์ฉ ๋ฐฉ๋ฒ์ <a href='https://formik.org/docs/overview#the-gist'>Getting Started</a>์๋ ๋์ ์์ง๋ง, ๋ด๊ฐ ์ง์  ๋ง์ฃผํ๋ ์ฝ๋์ ์ /ํ ์ํ๋ฅผ ๋ณด๋ฉด์ ์ค๋ชํ๋ ๊ฒ์ด ์๋ฌด๋๋ ๊ธฐ์ต์ ์ค๋ ๋จ์๊ฒ ๊ฐ์ ์ฝ๋๋ฅผ ์ผ๋ถ ์์ ํ์ฌ ๊ฐ์ ธ์๋ค.

์๋ ์ฝ๋๋ธ๋ญ์ `Formik` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ฌ์ฉํ๊ธฐ ์ ์ ์ฝ๋์ด๋ค. ์๋ง `React`๋ฅผ ์ฌ์ฉํ๋ ๊ฐ๋ฐ์๋ค์๊ฒ ์ต์ํ ์ฝ๋์ด์ง ์์๊น ์ถ๋ค. ์ฝ๋๋ฅผ ๋๋ต์ ์ผ๋ก ์ค๋ชํ์๋ฉด, `userEmail`๊ณผ `userName` ๋ชจ๋ `string` ํ์์ด๋ผ ํ๋์ `useState`์ ๋ฌถ์ด์ ์ ์ธํ๊ณ , `personalInfo`, `adInfo` ๋ชจ๋ `boolean` ํ์์ด๋ผ ๋ง์ฐฌ๊ฐ์ง๋ก ํ๋์ `useState`์ ์ ์ธํ๋ค. ๊ทธ๋ฆฌ๊ณ  `handleInput`์ `string`์ด ๋ณํ๋ ๊ฐ์ ์ถ์ ํ๋ ํจ์์ด๊ณ , `handlePartTerms`๋ `boolean` ํ์์ด ๋ณํ๋ ๊ฐ์ ์ถ์ ํ๋ ํจ์, `handleTotalTerms`๋ `boolean` ๊ฐ์ ํ๋ฒ์ ๋ชจ๋ ๋ฐ๊ฟ์ฃผ๋ ํจ์์ด๋ค. ํฌ๊ฒ๋ณด๋ฉด `handlePartTerms`์ `handleTotalTerms`์ ์ฑ๊ฒฉ์ด ๊ฐ์ ๋ฌถ์ด์ ์ ์ธํ  ์๋ ์์ง๋ง ํผ์ ํ๋๋ฅผ ์ง์  ์์ ํ๋ ๊ฒ๊ณผ ๋ ์์ชฝ ๋ ์ด์ด์์ ์์ ํ๋ ๊ฒ์ด ์ฐจ์ด๋ผ๋ ๊ด์ ์์ ๋ณด๋ฉด ๋๋๋ ํธ์ด ์ข์ ๊ฒ ๊ฐ์ ํธ๋ค๋ฌ๋ฅผ ๋ฐ๋ก ์ ์ํ๋ค. ๋ง์ง๋ง์ผ๋ก `handleSubmit`์ `form`์์ ์ ์ถ ํ  ๋ ์คํ๋๋ ํจ์์ด๋ค. ์ง๊ธ ์์ฑํ ์ฝ๋๋ณด๋ค ๋์ฑ ๊น๋ํ๊ฒ ์์ฑ ํ  ์ ์๋ ๋ฐฉ๋ฒ์ ๋ฌด๊ถ๋ฌด์งํ์ง๋ง, ํ์ฌ ์ํ์์๋ ํธ๋ค๋ฌ ์ฝ๋๊ฐ ๋ง์ ๊ฐ๋์ฑ์ด ๋จ์ด์ง๋ค๊ณ  ์๊ฐ์ด๋ ๋ค.

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

๊ทธ๋ ๋ค๋ฉด, `Formik` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ฌ์ฉํ ์ฝ๋๋ ์ด๋ป๊ฒ ์์ฑํ๋์ง ์ดํด๋ณด์. ํ๋จ ์ฝ๋๋ธ๋ญ์ ๋ณด๋ฉด ํ๋์ ๋ด๋ ์ฝ๋๊ฐ ๋ง์ด ์ค์ด๋  ๊ฒ์ด ๋ณด์ด์ง ์๋๊ฐ? ๋ํ, ๊ตฐ๋ฐ๊ตฐ๋ฐ ํฉ์ด์ ธ ์๋ ์ฝ๋๊ฐ ํ๊ณณ์ ๋ชจ์ฌ ์ ์ธ๋์ด์์ผ๋ ๊ฐ๋์ฑ๋ ๋ง์ด ์ข์์ก๋ค. ๊ทธ๋ฆฌ๊ณ  ์ด์  ์ฝ๋์์๋ ์ ํจ์ฑ ๊ฒ์ฌ ์ฝ๋๋ฅผ ๋ฃ์ง ์์๋๋ฐ, ์ฌ๊ธฐ์  `validationSchema`์ด๋ผ๋ ์ ํจ์ฑ ๊ฒ์ฌ ์ฝ๋๊น์ง ๊ฐ์ด ๋ฃ์๋ค.(`Yup` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ก ์ฌ์ฉํ๋ค.) `Formik`์ ์ฅ์ ์ `handleChange` ํธ๋ค๋ฌ๋ฅผ ํ์๋ณ(`string`, `boolean`)๋ก ๋ฐ๋ก ์ ์ํ์ง ์์๋ `handleChange` ํ๋๋ก ์ฌ์ฌ์ฉ ํ  ์ ์๋ค๋ ์ ๊ณผ ์ ํจ์ฑ ๊ฒ์ฌ๋ฅผ ์งํํ  ๋ `isValid`๋ฅผ ์ฌ์ฉํ์ฌ ์ ํจ์ฑ ๊ฒ์ฌ๊ฐ ๋ชจ๋ ํต๊ณผํ์ง ์์ผ๋ฉด(์ด๋๋ `isValid`๊ฐ `false`๊ฐ ๋๋ค.) `button`์ `disabled`๋ก ๋ง๋ค ์ ์๋ค. ์ถ๊ฐ๋ก `input`์ ์ต์ / ์ต๋ ๊ธธ์ด๊น์ง ์ ์ธํ์ฌ ํด๋น ๋ฒ์๋ฅผ ๋ฒ์ด๋๋ฉด ์๋ฌ๋ฉ์์ง๋ ๋์ธ ์ ์๋ค. ๊ทธ๋ฆฌ๊ณ  `errors` ๊ฐ์ฒด๋ก `validationSchema`์ ์ ํจ์ฑ ๊ฒ์ฌ๊ฐ ํต๊ณผํ์ง ์์์ ๋ ํด๋น ๋ฉ์ธ์ง๋ ๋ณด์ฌ์ค ์ ์๋ค. ๋ง์ง๋ง์ผ๋ก `form`์ ์ ์ถํ๊ณ  ์๋ต์ด ์ค๊ธฐ์ ๊น์ง `button`์ `disabled`๋ก ๋ง๋๋ `isSubmitting`๊น์ง ๋ณ๋์ ๋ก์ง ์ ์ธ์์ด ์ฌ์ฉํ  ์ ์๋ ์ ์ด ๋ง์์ ๋ค์๋ค.

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
        .email('์ด๋ฉ์ผ ํ์์ด ์๋๋๋ค.')
        .required('์ด๋ฉ์ผ์ ์๋ ฅํด ์ฃผ์ธ์.'),
      userName: Yup.string().required('๋๋ค์์ ์๋ ฅํด ์ฃผ์ธ์.'),
      personalInfo: Yup.bool().isTrue('๊ฐ์ธ์ ๋ณด ์์ง์ ๋์ํด์ฃผ์ธ์.'),
      adInfo: Yup.bool().isTrue('๊ด๊ณ ์ฑ ์ ๋ณด ์์ ์ ๋์ํด์ฃผ์ธ์.'),
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

์ด๋ฐ์๋ `React Context`์ ๋์ผํ๊ฒ ์ฌ์ฉ๊ฐ๋ฅํ `useFormikContext`, `form`ํ๊ทธ์ `handleSubmit`์ ์ฌ์ฉํ์ง ์์๋ ๋๋ `<Form />`, `input` ํ๊ทธ์ `handleChange`๋ฅผ ์ฌ์ฉํ์ง ์์๋ ๋๋ `<Field />`ํ๊ทธ ๋ฑ ๋ค์ํ ๊ธฐ๋ฅ์ด ์๋ค. ๊ทธ๋ฌ๋, `Formik`์๋ ๋จ์ ์ ์กด์ฌํ๋ค. ์ ํด์ง ํ๊ทธ๋ฅผ ์ฌ์ฉํด์ผํ๋ ์ , ๋ณต์กํ `form`์ ๋ค๋ฃจ๊ธฐ๊ฐ ์ด๋ ต๊ณ  ๋ด๊ฐ ์ํ๋๋๋ก ์ปค์คํํ๊ธฐ๊ฐ ์ผ๋ฐ์ ์ธ `form`์ ๋นํด ์กฐ๊ธ์ ๋ถํธํ๋ค๋ ์ . ๊ทธ๋ฌ๋, ์ด๋ฐ ๋จ์ ์ ๊ทน๋ณตํ  ์ ์๋ ์ฅ์ (`input` ๋ก์ง์ ์ผ์ผ์ด ๊ตฌํํ๊ธฐ ๊ท์ฐฎ๊ฑฐ๋ ํ์ํ  ๋ ๋ฑ)์ด ๋ ๋ง๊ธฐ ๋๋ฌธ์ ํ๋ฒ์ฏค `Formik`์ ์ฌ์ฉํ๋ ๊ฒ์ ๊ถํ๋ฉฐ ์ด ๊ธ์ ๋ง์น๋ค.   

Reference
1. https://formik.org/docs/overview
2. https://github.com/jquense/yup
3. https://formik.org/docs/overview#the-gist
4. https://formik.org/docs/migrating-v2#isvalid
5. https://formik.org/docs/api/form
6. https://formik.org/docs/api/field
7. https://react-hook-form.com/

---
## ๐ 2๊ฐ ์ด์์ ๋์ผํ ๊ธฐ๋ฅ์ธ useState์ handleChange๋ฅผ ๊ฐ๊ฐ ํ๋๋ก ๊ด๋ฆฌํ๊ธฐ
๊ตฌ๋์ ์ฒญ ํ์ด์ง๋ฅผ ๋ง๋ค๋ฉด์ ๊ฐ์ธ์ ๋ณด์์ง checkbox์ ๊ด๊ณ ์ฑ ์ ๋ณด ์์  `input checkbox`๋ฅผ `useState`๋ก ๊ด๋ฆฌํ๋ ค๊ณ  ํ๋๋ฐ, ํ ์ค์ฉ `useState`๋ก ์ ์ธํ์ฌ ๊ด๋ฆฌํ๋๊น ์ฝ๋๊ฐ ๋ถํ์ํ๊ฒ ๊ธธ์ด์ ธ ์ข์ ์ฝ๋๋ผ๊ณ  ๋ณด๊ธฐ ํ๋ค์๋ค. ๊ทธ๋์ ๊ธฐ์กด์ `const [state, setState] = useState(false)`์ฒ๋ผ ์ ์ธํ๋ค๋ฉด ์ด๋ฅผ `object`๋ก ๋ฌถ์ด ์ ์ธํ๋ ์ฝ๋๊ฐ ์ค์ด๋ค์๋ค. 

```typescript
import { useState } from "react";

// ๋ฆฌํฉํ ๋ง ์ 
export default function Subscription() {
  const [allAgree, setAllAgree] = useState<boolean>(false);
  const [privateAgree, setPrivateAgree] = useState<boolean>(false);
  const [adAgree, setAdAgree] = useState<boolean>(false);
}


// ๋ฆฌํฉํ ๋ง ํ
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

๋ํ, `checkbox`์ `state`๋ฅผ ๋ณ๊ฒฝํ๊ธฐ ์ํด `setAgree`์ ๊ฐ์ ๋ณ๊ฒฝํด์ผํ๋๋ฐ, ๋ฆฌํฉํ ๋ง ์ ์๋ `allAgree`๋ง์ ์ํ `allHandleChange`๋ฅผ ๋ง๋ค์๊ณ , ๋๋จธ์ง `privateAgree`, `adAgree`๋ฅผ ์ํ `handleChange` ํจ์๋ฅผ ๋ง๋ค์๋๋ฐ, ๋ง์ฐฌ๊ฐ์ง๋ก ๋ณ์๋ช๋ง ๋ค๋ฅผ๋ฟ ๋์ผํ ์ญํ ์ ํ๋๋ฐ๋ ํจ์๋ฅผ 2๊ฐ ์ด์ ๊ด๋ฆฌํด์ผํ๊ธฐ ๋๋ฌธ์ ๊ฐ๋์ฑ์ด ๋งค์ฐ ์ข์ง ์์๊ณ , ํจ์ 1๊ฐ๋ก ๊ด๋ฆฌํ๊ธฐ ์ํด `input`์ `id`๊ฐ์ผ๋ก ์กฐ๊ฑด์ ๊ตฌ๋ถํ์ฌ `check`๋ฅผ `toggle` ํ  ์ ์๊ฒ ์ค์ ํ๋ค. ๊ทธ๋ฆฌ๊ณ  `์ ์ฒด ๋์ํฉ๋๋ค` ๊ธฐ๋ฅ์ ํด๋น `input`์ `checked` property์ `personalInfo`, `adInfo`๊ฐ ์ฐธ์ผ ๋๋ง `checked`๊ฐ ๋  ์ ์๊ฒ ์ค์ ํ๋ค. ์ด๋ ๊ฐ๋์ฑ์ ์ํด ๋ชจ๋ ๋์ ํ์ ๋์ ์กฐ๊ฑด์ `const checkedAllInfo = personalInfo && adInfo` ๋ณ์๋ก ๊ด๋ฆฌํ๋ค. ์ฌ๋ด์ผ๋ก 66๋ฒ ๋ผ์ธ์ `else`๋ก ์ฒ๋ฆฌ ํ  ์ ์์ง ์๋๊ฐ๋ผ๊ณ  ์๊ฐํ  ์ ์์ง๋ง, 7๋ฒ ๋ผ์ธ์ `AgreeType`ํ์์ ์ ์ํด๋์๊ธฐ ๋๋ฌธ์ `else` ์กฐ๊ฑด์ `personalInfo` ํน์ `adInfo` ์ธ์ ๋ค๋ฅธ `id`๊ฐ ์จ๋ค๋ฉด ํ์์คํฌ๋ฆฝํธ์์ ์ ๋๋ก ์ฒ๋ฆฌ ํ  ์ ์๋ค. ๋ง์ฝ, `else`๋ฅผ ๋ฃ๊ณ  ์ถ๋ค๋ฉด 7๋ฒ๋ผ์ธ์ `AgreeType`์ `[key in string]: boolean`์ ๋ฃ์ด ๋ค๋ฅธ `id`๊ฐ ์ค๋๋ผ๋ ๊ทธ `id`๊ฐ `boolean` ํ์์ด๋ผ๊ณ  ์ ์ํ๋ฉด ๋๋ค.

```typescript
import { useState } from "react";

// ๋ฆฌํฉํ ๋ง ์ 
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
          ์ ์ฒด ๋์ํฉ๋๋ค.
        </label>

        <label htmlFor="agree-private">
          <input
            id="agree-private"
            type="checkbox"
            checked={privateAgree}
            onChange={() => handleChange(setPrivateAgree)}
          />
          ๊ฐ์ธ์ ๋ณด ์์ง ๋ฐ ์ด์ฉ์ ๋์ํฉ๋๋ค. (ํ์)
        </label>

        <label htmlFor="agree-advertise">
          <input
            id="agree-advertise"
            type="checkbox"
            checked={adAgree}
            onChange={() => handleChange(setAdAgree)}
          />
          ๊ด๊ณ ์ฑ ์ ๋ณด ์์ ์ ๋์ํฉ๋๋ค. (ํ์)
        </label>

        <Button disabled={!(privateAgree && adAgree)}>๊ตฌ๋ํ๊ธฐ</Button>
      </Section>
    </>
  );
}

// ๋ฆฌํฉํ ๋ง ํ 
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
            ์ ์ฒด ๋์ํฉ๋๋ค.
          </Text>
        </label>

        <label htmlFor="personalInfo">
          <input
            id="personalInfo"
            type="checkbox"
            checked={personalInfo}
            onChange={handleChange}
          />
          <Text size="medium">๊ฐ์ธ์ ๋ณด ์์ง ๋ฐ ์ด์ฉ์ ๋์ํฉ๋๋ค. (ํ์)</Text>
        </label>

        <label htmlFor="adInfo">
          <input
            id="adInfo"
            type="checkbox"
            checked={adInfo}
            onChange={handleChange}
          />
          <Text size="medium">๊ด๊ณ ์ฑ ์ ๋ณด ์์ ์ ๋์ํฉ๋๋ค. (ํ์)</Text>
        </label>

        <Button disabled={!checkedAllInfo}>๊ตฌ๋ํ๊ธฐ</Button>
      </Section>
    </>
  );
}
```

![](https://velog.velcdn.com/images/abcd8637/post/09e69e6f-01b7-49a8-bfb5-adb3cf3af32d/image.gif)

---
## ๐ interface, type ์ฌ์ฉํ๊ธฐ

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
## ๐ ๋ฐฐ์ดํ์ ์ ์ธํ๊ธฐ
1. ์ฒซ๋ฒ์งธ ๋ฐฉ๋ฒ: ์ ๋ค๋ฆญ์ type์ ๋ณ์๋ฅผ ์ ๊ณตํ๋ ๋ฐฉ๋ฒ์ด๋ฉฐ ์ ๋ค๋ฆญ์ด ์์ผ๋ฉด ์ด๋ค ๊ฒ์ด๋  ํฌํจ ํ  ์ ์๋ค.
2. ๋๋ฒ์งธ ๋ฐฉ๋ฒ: ๋ฐฐ์ด ์์๋ค์ ๋ํ๋ด๋ ํ์ ๋ค์ `[]`๋ฅผ ์ฌ์ฉํ๋ค.

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
๐๐ฝ ['father', 'mother', 'brother'], ['father', 'mother', 'brother']

console.log(myFamilyAges, myFamilyAges2)
๐๐ฝ [50, 48, 27, 25], [50, 48, 27, 25]

console.log(myInfo, yourInfo)
๐๐ฝ [{address: 'HWASEONG'}], [{address: 'HWASEONG'}]
```

---
## ๐ interface ์ฌ์ฉํ์ง ์๊ณ  ์จ๋ณด๊ธฐ ์ฌ์ฉํ๊ณ  ์จ๋ณด๊ธฐ

```ts
// do not use Interface
const getMathScore = (scoreObj: {math: number}): number => {
    return scoreObj.math
}

const myScore = {korean: 85, math: 95, english: 87}
const myMathScore = getMathScore(myScore);

console.log(myMathScore)
๐๐ฝ 95

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
๐๐ฝ 92
```

---
## ๐ TodoListItem Component ์์ฑ ํ props๋ก constant ๋ณด๋ด๊ธฐ

1. `TodoListItem` ์ปดํฌ๋ํธ๋ฅผ ์์ฑํ๋ค.
   
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
## ๐ ํ์ ์ฃผ์๊ณผ ํ์ ์ถ๋ก 
`TS`์์ `1๋ฒ` ์ฝ๋์ฒ๋ผ ์ฝ๋ก (`:`)๊ณผ ํ์์ ๋ถ์ด๋๋ฐ ์ด๋ฅผ `ํ์ ์ฃผ์(type-annotation)`์ด๋ผ๊ณ  ํ๋ค. ๋ฐ๋ฉด `2๋ฒ` ์ฝ๋์ฒ๋ผ ํ์ ๋ถ๋ถ์ด ์๋ต๋๋ฉด ๋์ ์ฐ์ฐ์ ์ค๋ฅธ์ชฝ ๊ฐ์ ๋ถ์ํด ์ผ์ชฝ ๋ณ์์ ํ์์ ๊ฒฐ์ ํ๋๋ฐ ์ด๋ฅผ `ํ์ ์ถ๋ก (type interface)`๋ผ๊ณ  ํ๋ค. ์ด ๊ธฐ๋ฅ์ `JS`์ ํธํ์ฑ์ ๋ณด์ฅํ๋๋ฐ ํฐ ์ญํ ์ ํ๋ค. ํ์ ์ถ๋ก  ๋๋ถ์ `.js` ํ์ผ์ `.ts`๋ก ๋ฐ๊ฟ๋ `TS` ํ๊ฒฝ์์๋ ๋ฐ๋ก ๋์ํ๋ค.

```ts
let number: number = 1;    // ํ์ ์ฃผ์(type annotation)
let testInterface = 3;    // ํ์ ์ถ๋ก (type interface)

testInterface = '123';    // type ๋ถ์ผ์น ์ค๋ฅ
```

---
## ๐ vscode์์ Cannot use JSX unless the '--jsx' flag is provided ๋ฐ ๋
์ฒ์ `TS`๋ฅผ ์ค์นํ๊ณ  ์์์ ํ๋ ๋์ค `yarn start`๋ฅผ ์น๋๊น ๊ฐ์๊ธฐ `tsx`ํ์ผ์ ๋นจ๊ฐ ์ค์ด ๋จ๋ฉด์ ์ ๋ชฉ๊ณผ ๊ฐ์ ๋ฌธ๊ตฌ๊ฐ ๋์๋ค. ๊ฒฐ๋ก ์ ์ผ๋ก `TS` ๋ฒ์ ์ด ๋ง์ง ์์ ์๊ธฐ๋ ์ค๋ฅ์ธ๋ฐ ์ ์ผ ๊ฐ๋จํ ๋ฐฉ๋ฒ์ `VScode` ํ๋จ์ ์ต์  ๋ฒ์ ์ผ๋ก ์ ์ฉํ๋ ๋ฐฉ๋ฒ์ด๋ค.

1. ์ฐ์ธก์์ ๋ค๋ฒ์งธ ๋ฒ์ ์ ํด๋ฆญํ๋ค.

![](https://images.velog.io/images/abcd8637/post/eb40eaff-5764-41ee-8c7f-96389c0fcc7f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2010.55.21.png)

2. ์์์ ๋๋ฒ์งธ `Select TypeScript version...`์ ํด๋ฆญํ์. 

![](https://images.velog.io/images/abcd8637/post/9dc25671-6e48-45e3-8e5e-e231f166f11b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2010.51.46.png)

3. ์ต์  ๋ฒ์ ์ผ๋ก ๋๋ฌ์ฃผ๋ฉด ๋๋ค.

![](https://images.velog.io/images/abcd8637/post/fe2916a3-9ef1-4926-87c4-a134bb112a8f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2010.51.54.png)

4. ์ ์ฉ ์ฑ๊ณต

![](https://images.velog.io/images/abcd8637/post/f3a7a334-b19b-415a-9eef-6951f9bf8aa2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2010.52.05.png)

๊ทธ๋ฌ๋ ๋ฒ์ ์ด ๋จ์ง ์์ ์ ์ฉ์ด ์๋๋ฉด ๋ค์๊ณผ ๊ฐ์ด ์๋ก ์ค์นํด์ฃผ์.

1. `terminal`์ `npm install -g typescript`๋ฅผ ์๋ ฅํ๋ค.

2. `npm list -g typescript`๋ฅผ ์๋ ฅํด ์ด๋ค ๊ฒฝ๋ก์ ์ค์น๋์๋์ง ํ์ธํ๋ค.

![](https://images.velog.io/images/abcd8637/post/577eefc4-ac11-4994-9791-2e696df548fa/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2010.59.12.png)

3. `vscode`์์ `settings.json`ํ์ผ์ ๋ค์ด๊ฐ๋ค(MAC์ `command + shift + p`)

![](https://images.velog.io/images/abcd8637/post/1d991a45-6096-4632-ab13-1a18ffb5ce1e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2011.02.35.png)

4. `"typescript.tsdk": "2๋ฒ์์ ํ์ธํ ๊ฒฝ๋ก/node_modules/typescript/lib"`์ ์ถ๊ฐํ๋ค.

![](https://images.velog.io/images/abcd8637/post/04689df6-f89d-4c11-8302-9c8eada3edc8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-10%2011.03.13.png)

5. ๋งจ ์์ ๋ฐฉ๋ฒ์ ๋ค์ํด๋ณธ๋ค.(๋ฒ์  ํด๋ฆญ -> ์ต์  ๋ฒ์  ์๋ ฅ)

6. Happy Hacking โจ

> reference: <a href='https://chacha73.tistory.com/44'>Tistory</a>, <a href='https://stackoverflow.com/questions/50432556/cannot-use-jsx-unless-the-jsx-flag-is-provided'>stackoverflow</a>

---
## ๐ form ํ๊ทธ๋ฅผ ์ด์ฉํ์ฌ state ๊ด๋ฆฌํ๊ธฐ
`TS`๋ฅผ ์ด์ฉํ์ฌ `Todo-list`๋ฅผ ๋ง๋ค๋ค๊ฐ ์ด์ ๊น์ง๋ `button`์ `onClick`์ ์ด์ฉํ์ฌ `input` ๊ฐ์ ๋ณด๋๋๋ฐ ์ด๋ฒ์๋ `form`ํ๊ทธ๋ฅผ ์ฌ์ฉํ์ฌ ๊ฐ์ ๋ณด๋ด๋ดค๋ค. `form`ํ๊ทธ๋ฅผ ์ฌ์ฉํ๋ฉด `input`์ฐฝ์ ๊ฐ์ ๋ณด๋ผ ๋ `onKeyPress(enter)` ํจ์๋ฅผ ๋ฐ๋ก ๋ง๋ค์ง ์์๋ ๋ผ์ ์ ์ฉํ๋ค. ์ฌ๋งํ๋ฉด `useState`, `useRef`๊ฐ ์ด๋ค ํ์์ธ์ง๋ ๋ช์ํ์.

๋, ํจ์๋ด์์ `event`๋ฅผ ๋ฐ์์ฌ๋ `eventType`์ด ๋ฌด์์ธ์ง ์๊ณ  ์ถ์ผ๋ฉด `event`๋ฅผ ์ฒ๋ฆฌํ๋ ํ๊ทธ์ ๋ง์ฐ์ค๋ฅผ ์ฌ๋ฆฌ๋ฉด ํ์์ ์ด๋ป๊ฒ ์ ์ฉํด์ผํ๋์ง ๋ค์ ์ฌ์ง์ฒ๋ผ ํ์์ฐฝ์ด ๋ฌ๋ค.

![](https://images.velog.io/images/abcd8637/post/72030866-30f0-412e-a5db-49f70fb94335/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-20%2022.19.48.png)

ํ์ผ๋ก `15๋ฒ ๋ผ์ธ`์ฒ๋ผ ํจ์๋ด์์ ํจ์๋ฅผ ์ ์ธ ํด์ผํ๋ ๊ฒฝ์ฐ ๊ฐ๋์ฑ์ ๋์ด๊ธฐ ์ํด `utility` ํด๋๋ฅผ ๋ฐ๋ก ๋ง๋ค์ด์ ์ ์ฅํ๊ณ  `import` ํ๋ ๋ฐฉ๋ฒ์ ์ฌ์ฉํ๋ค.

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
                    placeholder="๊ฐ์ ์๋ ฅํ์ธ์."
                    onChange={handleEvent}
                    value={title}
                />
                <button>์ถ๊ฐ!</button>
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
## ๐ customHooks๋ก getApi ์ฝ๋ ๊ด๋ฆฌํ๊ธฐ
`fetch` ํน์ `axios`๋ฅผ ์ฌ์ฉํ์ฌ `api`๋ฅผ ๋ฐ์์ฌ ๋ ๊ธฐ์กด์๋ `app.tsx` ํ์ผ๋ด์์ `useEffect`๋ฅผ ์ฌ์ฉํ์ฌ `api`๋ฅผ ๋ฐ์์ค๋ ์ฝ๋๋ฅผ ๋ชจ๋ ์์ฑํ๋ค. ๊ทธ๋ฌ๋, `app.tsx`์ `api`์ ๊ด๋ จ๋ ์ฝ๋๋ฅผ ๋ชจ๋ ์์ฑํ๋ฉด ๋ค๋ฅธ ๋ก์ง์ฝ๋๋ฅผ ๋ณด๋๋ฐ ์ ๊ฒฝ์ฐ์ฌ์ ๋ค๋ฅธ๊ณณ์ผ๋ก ์ฎ๊ฒจ์ผ๊ฒ ๋ค๋ ์๊ฐ์ด ๋ ์ฌ๋๊ณ  `hooks` ํด๋๋ฅผ ์์ฑํ์ฌ `customHooks`๋ก ๋ฐ๋ก ๊ด๋ฆฌํ๋ ๋ฐฉ๋ฒ์ด `app.tsx`์ ๊ฐ๋์ฑ์ ์ฆ๊ฐ์ํจ๋ค๊ณ  ์๊ฐํ๋ค.

`data`, `loading`, `error` ๊ฐ์ `export` ํ  ๋ ํ์์ถ๋ก ์ด ๋ชํํ๊ฒ ๋์ง ์๋๊ฒฝ์ฐ๊ฐ ์์ด `as`๋ฌธ์ ์ฌ์ฉํด์ ์ด๋ค ํ์์ธ์ง ๋ช์ํ๋ค. `as`๋ฌธ ๋์  `interface`๋ฅผ `usePosts` ๋ฆฌํด๊ฐ์ ๋ถ์ฌ์ค๋ ๋๋๋ฐ ์ด๋๋ `{}`๋ก `return` ํ๊ณ  `app.tsx`์์ ๋ถ๋ฌ์ฌ ๋๋ `{}`๋ก ๋ถ๋ฌ์ค๋๊ฒ์ ์์ง๋ง์.

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
## ๐ useLayoutEffect, useEffect์ ์ฐจ์ด์  ์์๋ณด๊ธฐ
`react`๋ฅผ ์ฌ์ฉ ํ  ๋ `paint screen` ๋จ๊ณ ์ดํ ์ฆ, ์ปดํฌ๋ํธ๊ฐ ํ๋ฉด์ ๋ ๋๋ง ๋ ์ดํ `์ฌ์ด๋ ์ดํํธ(side Effect)` (์ฌ๊ธฐ์ ๋งํ๋ ์ฌ์ด๋ ์ดํํธ๋ ์ปดํฌ๋ํธ ๋ด์์ ๋ฐ์ดํฐ ๊ฐ์ ธ์ค๊ธฐ, ๊ตฌ๋ ์ค์ ํ๊ธฐ, ์๋์ผ๋ก ์ปดํฌ๋ํธ์ DOM์ ์กฐ์ํ๋ ์์ ๋ฑ์ ๋งํ๋ค.)๋ฅผ ๋น๋๊ธฐ์ ์ผ๋ก ํธ์ถํ  ๋ ์ฃผ๋ก `useEffect`๋ฅผ ์ฌ์ฉํ๋ค. 

์๋ ์ฌ์ง์ <a href='https://github.com/donavon/hook-flow'>github</a>์์ ๊ฐ์ ธ์จ `hook-flow` ์ฌ์ง์ธ๋ฐ, ํ๋์ ์ ์ ์์ด์ ๊ฐ์ ธ์๋ดค๋ค.

![](https://images.velog.io/images/abcd8637/post/2cb835c9-eb26-4d1b-bd14-711c8a1695d5/hook-flow.png)

๋ค์ ๋ณธ๋ก ์ผ๋ก ๋์๊ฐ์ ํ๋ฉด์ `state`์ ๊ฐ์ด ๋ฐ๋์ด์ ํ๋ฉด์ด ๊น๋นก์ด๊ฑฐ๋ `DOM`์ ํธ์ถํ๊ธฐ ์ ์ ๋ฌด์ธ๊ฐ๋ฅผ ๋ณ๊ฒฝํ๊ณ  ์ถ์ ๋๋ ์ด๋ป๊ฒ ํด์ผํ ๊น? `paint screen` ๋จ๊ณ ์ด์  ์ฆ, ํ๋ฉด์ด ๊ทธ๋ ค์ง๊ธฐ ๋ฐ๋ก ์ ์ ๊ฐ์ ๋๊ธฐ์ ์ผ๋ก ํธ์ถํ๊ณ  ์ถ๋ค๋ ์๊ฐ์ด ๋ค๋ฉด `useLayoutEffect` hook์ ์ฌ์ฉํ๋ฉด ๋๋ค.

์ ๋ง๋ก `useLayoutEffect`๋ ํ๋ฉด์ ๊ทธ๋ ค์ง๊ธฐ ์ ์ ์คํ๋๋์ง ๊ถ๊ธํ์ฌ ๊ฐ๋จํ ์คํ์ ์งํํ๋ค. ๋ง์ฝ, `localStorage`์ ์ ์ฅ๋ ๊ฐ์ ๋ถ๋ฌ์ค๋ ์์์ ์ฌ์ด๋์ดํํธ๋ก ๊ด๋ฆฌํ๋ค๊ณ  ํ์ ๋, `useEffect`, `useLayoutEffect` ์ค ์ด๋ค ์ฝ๋๋ก ์์ฑํด์ผ ์ ํฉํ ๊น?

์๋ชจ๋ฅด๊ฒ ๋ค๋ฉด ํ๋จ์ `gif`๋ฅผ ๋ณด๊ณ  ์ด๋ค `gif`๊ฐ `useEffect` ํน์ `useLayoutEffect`๋ฅผ ์ฌ์ฉํ์์ง ํ๋ฒ ๊ณ ๋ฏผํด๋ณด์.

![](https://images.velog.io/images/abcd8637/post/2bba6e7a-a5e7-443b-8b84-349e5a58b8d4/useLayoutEffect.gif)

![](https://images.velog.io/images/abcd8637/post/35c4d223-5afc-4b52-91cb-ecd43f5cfbdf/useEffect.gif)

๋๋ฌด ์ฌ์ ๋๊ฐ? ์ ๋ต์ ์ฒซ๋ฒ์งธ `gif`๊ฐ ๋ฐ๋ก `useLayoutEffect`๋ฅผ ์ฌ์ฉํ ์ฝ๋์ด๊ณ  ๋๋ฒ์งธ `gif`๋ `useEffect`๋ฅผ ์ฌ์ฉํ ์ฝ๋์ด๋ค. ์ด๋ค ์ฐจ์ด์ ์ด ์๋์ง ๋์ ํฌ๊ฒ ๋จ๊ณ  ๋ณด๋ฉด ๋ณด์ผํ๋ฐ, ๋๋ฒ์งธ `gif`๋ ํ๋ฉด์ ๋ ๋๋ง ๋ ์ดํ์ `localStorage` ๊ฐ์ ๊ฐ์ ธ์ค๊ธฐ ๋๋ฌธ์ ์ฒ์์๋ ๋น ๊ณต๊ฐ์ด์๋ค๊ฐ ๋์ค์ ๊ฐ์ด ์ฑ์์ง๋๊ฒ์ ์ ์ ์๊ณ  ๋ฐ๋ฉด์, ์ฒซ๋ฒ์งธ `gif`๋ ๋ธ๋ผ์ฐ์ ์ ๊ทธ๋ ค์ง๊ธฐ ์ ์ ๋ก์ง์ ์คํํ๋ฏ๋ก ๋ง๋ํ๊ฒ ๊ฐ์ ธ์ค๋๊ฒ์ ์ ์ ์๋ค.

์ด๋ฐ ์ธ์ธํ ๋ถ๋ถ๊น์ง ์ ๊ฒฝ์จ์ผํด..?๋ผ๋ ์๊ฐ์ด ๋ค ์๋ ์๋ค. ๊ทธ๋ฌ๋ ์คํ๊ณผ ๊ฐ์ด ๊ฐ์ด ์์ ๊ฒฝ์ฐ์๋ ๋ฏธ๋ฏธํ  ์ ์์ง๋ง ์ค์  ๋ฐฐํฌ ์ดํ ๊ฐ์ด ์ปค์ก์ ๋, ๋ธ๋ผ์ฐ์ ๊ฐ ์กฐ๊ธ์ด๋ผ๋ ๋๋ ค์ง๊ฒ ๋๋ค๋ฉด ๊ณ ๊ฐ์ ๊ฒฝํ์ฑ์ด ์ข์ง ์์ ์ ์๋ค. ๋จ๋ค๊ณผ ๊ฒฝ์๋ ฅ์๋ `front-end`๊ฐ ๋๋ ค๋ฉด ์ด๋ฐ ๋ํ์ผํ ๋ถ๋ถ๊น์ง ์บ์น ํ  ์ ์์ด์ผ ํ๋ค๊ณ  ์๊ฐํ๋ค.

reference
1. <a href='https://ko.reactjs.org/docs/hooks-effect.html'>react-docs-hooks-effect</a>

---
## ๐ object(key:value) type, interface ์ ์ธํ๊ธฐ
`TS`๋ฅผ ์ฌ์ฉํ๋ค๊ฐ `string`, `number`, `boolean`, `array`์ ๊ฐ์ ์๋ฃํ์๋ `type`์ ์ ์ ์ธํ  ์ ์์๋๋ฐ, ์ ๋ `key:value`์ ๊ฐ์ `object`๋ `type`์ ์ด๋ป๊ฒ ์ ์ธํด์ผํ๋์ง ํท๊ฐ๋ ธ๋ค. ๋๋ ์ด๋๊น์ง `key:value`ํํ์ `object`๋ ๋ค์๊ณผ ๊ฐ์ด ์ ์ธํ๋ค.

```ts
interface PostType{
    sort: string;
}

const post: PostType = {sort: "asc"}
console.log(post);
๐๐ฝ { sort: 'asc' } 
```

`ReactQuery`๋ฅผ ์ฌ์ฉํ์ฌ `useInfiniteQuery`๋ฅผ ๊ตฌํํ๋ ๋์ค ํจ์ `parameter`๋ก ๋ฐํ๋๋ `key:value` ํํ์ธ `post`๊ฐ์ `key`๋ `type`๋ง ์๊ณ  `๊ฐ`์ ์ ํํ์ง ์์ ๋ ํน์ `post`๊ฐ์ `value`๋ `type`๋ง ์๊ณ  `๊ฐ`์ ์ ํํ์ง ์์ ๋, ํน์ `๋น๊ตฌ์กฐํ ๋น๋ฌธ๋ฒ(destructuring assignment)`์ ์ฌ์ฉํ์ ๋๋ `type`์ ์ด๋ป๊ฒ ์ ์ธํด์ผํ๋์ง ๊ถ๊ธํ๋ค. ๊ทธ๋์ ๋ฐฐ์๋ณด์. how to declare object type!! 

```ts
// post์ `key`๋ ์ ๋ชจ๋ฅด์ง๋ง `type`์ ํ์ค ํ  ๋
type SortType = {[key in string]: string};
const post = {sort: "asc"};
const {sort} = post as SortType;

// post์ `key` ๊ฐ์ ๋ช ๊ฐ ์๊ณ  ์์ ๋
type Keys = "sort" | "filter" 
type SortType = {[key in Keys]: string};
const post = {sort: "asc"};
const {sort} = post as SortType;

// post์ `value` ๊ฐ์ ๋ช ๊ฐ ์๊ณ  ์์ ๋
type Values = "asc" | "desc" | "id";
type SortType = {[key in string]: Values};
const post = {sort: "asc"};
const {sort} = post as SortType;

// post์ `key:value`๋ฅผ ๋๋ต์ ์ผ๋ก ์๊ณ  ์์ ๋
type Keys = "sort" | "filter" 
type Values = "asc" | "desc" | "id";
type SortType = {[key in Keys]: Values};
const post = {sort: "asc"};
const {sort} = post as SortType;

// post์ ์ ํํ `key:value` ๋ชจ๋ ํ์คํ ๊ฐ์ผ ๋
type SortType = {sort: string};
const post = {sort: "asc"};
const {sort} = post as SortType;

// arrayํํ๋ก return๋๋ post type ์ ์ธ
type PostObj = {[key in string]: string};
type PostType = Array<string | PostObj>
const post: PostType = ["usePosts", {sort: "asc"}];
```

์ง๊ธ๊น์ง ์์ฑํ ์ฝ๋๋ฅผ ํ ๋๋ก ์ดํด๋ณด๋ฉด `๋น๊ตฌ์กฐํ ๋น`์ `type`์ ์ธ์ ๋ณ์์์น๊ฐ ์๋ ํ ๋นํ๋ ๊ฐ ๋ค์ `as`ํํ๋ก ๋ถ์๋ค. ์ด๋ฅผ ํ๊ตญ๋ง๋ก ๋ค์ด ์บ์คํ ํน์ ์์ด๋ก `Type Assertions`์ด๋ผ๊ณ  ๋ถ๋ฅธ๋ค. ์ด์ ๊ด๋ จํ <a href='https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions'>TypeScriptLang</a>์ ๋์์๋ ์์ด๋ฅผ ๋ฒ์ญํ๋ฉด ๋ค์๊ณผ ๊ฐ์ ๊ธ์ ์ฝ์ ์ ์๋ค. ๋๋๋ก `TS`๊ฐ ์ ์ ์๋ ๊ฐ์ ์ ํ์ ๋ํ ์ ๋ณด๊ฐ ์์ ๊ฒ์๋๋ค. (...์ค๋ต...) ์ด๋ฌํ ์ํฉ์์ `Type Assertions`์ ์ฌ์ฉํ์ฌ ๋ณด๋ค ๊ตฌ์ฒด์ ์ธ ์ ํ์ ์ง์ ํ  ์ ์์ต๋๋ค. ๋ํ, `TypeScript Deep Dive`์๋ `TypeScript`์์๋ ์์คํ์ด ์ถ๋ก  ๋ฐ ๋ถ์ํ ํ์ ๋ด์ฉ์ ์ฐ๋ฆฌ๊ฐ ์ํ๋ ๋๋ก ์ผ๋ง๋ ์ง ๋ฐ๊ฟ ์ ์์ต๋๋ค. ์ด๋ "ํ์ ํ๋ช(type assertion)"์ด๋ผ ๋ถ๋ฆฌ๋ ๋ฉ์ปค๋์ฆ์ด ์ฌ์ฉ๋ฉ๋๋ค. TypeScript์ ํ์ ํ๋ช์ ํ๋ก๊ทธ๋๋จธ๊ฐ ์ปดํ์ผ๋ฌ์๊ฒ ๋ด๊ฐ ๋๋ณด๋ค ํ์์ ๋ ์ ์๊ณ  ์๊ณ , ๋์ ์ฃผ์ฅ์ ๋ํด ์์ฌํ์ง ๋ง๋ผ๊ณ  ํ๋ ๊ฒ๊ณผ ๊ฐ์ต๋๋ค. ์ฝ๊ฒ ๋งํด ๋ด๊ฐ ํ์์ ์ ์ธํ๋๊น ๋ด ๋ง๋๋ก ํด๊ฐ ๋๋ค.

reference
1. <a href='https://www.designcise.com/web/tutorial/how-to-specify-types-for-destructured-object-properties-using-typescript'>How to Specify Types for Destructured Object Properties Using TypeScript?</a>
2. <a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions">typescriptlang</a>
3. <a href="https://radlohead.gitbook.io/typescript-deep-dive/type-system/type-assertion">TypeScript Deep Dive</a>
## ๐ npm install ํ package-lock.json์ diff๊ฐ ๋ง์ ๋
๋ด๊ฐ ๋ด๋นํ `repository`๋ฅผ ์์ํ๊ธฐ ์ ์, ํฐ๋ฏธ๋์ `npm install`์ผ๋ก ํด๋น `project`๋ฅผ ์คํํ๊ธฐ ์ํด ํ์ํ ํจํค์ง๋ฅผ ์ค์นํ๋ ๋์ค `VSCode`์ `Source Control - Changes`์์ `main` ๋ธ๋์น์ `package-lock.json`์ ์ฝ๋์ ๋ค๋ฅด๊ฒ `diff`๊ฐ ์์ฒญ ๋ง์ด ์๊ธฐ๋ฉด์ `npm run dev`๋ฅผ ์คํํ๋ฉด ์ค๋ฅ๊ฐ ๋จ๋ฉด์ ์ ์์ ์ผ๋ก `local` ํ๊ฒฝ ์คํ์ด ์๋๋ ๊ฒฝ์ฐ๊ฐ ์์๋ค.

![](https://velog.velcdn.com/images/abcd8637/post/1e2defd0-55ec-4213-bcbf-ba265974c984/image.png)

`main` ๋ธ๋์น์ `package-lock.json`๊ณผ ๋ค๋ฅธ๊ฒ๋ ์๋๋ฐ ๊ฐ์๊ธฐ `diff`๊ฐ ๋ง์ด ์๊ฒจ์ ๋นํฉํ์ง๋ง, ์นจ์ฐฉ์ ์ ์งํ๋ฉฐ ๊ตฌ๊ธ๋ง์ ํ ๊ฒฐ๊ณผ ์์ธ์ `npm`์ด ํญ์ `package-lock.json`์์ ๊ฐ๋ฅํ ๋ชจ๋  ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์ค๋ ค๊ณ  ์๋ํ๋ ํน์ง ๋๋ฌธ์ ์๊ธด ๋ฌธ์ ์๋ค. ํ๋ก์ ํธ ํ์ผ ์ค `package-lock.json`์ ์ ๋ค์ฌ๋ค๋ณด๋ฉด 3๋ฒ์งธ ์ค์ `lockfileVersion`์ด๋ ์ฝ๋๊ฐ ์๋๋ฐ, ์ด๋ `document`์ ์ฒด๊ณ์ ์ธ ๋ฒ์ ์ ๋ํ๋ด๋ ์ฝ๋์ด๋ค. 

`lockfileVersion`์ `npm`์ ๋ฒ์ ์ ๋ฐ๋ผ 1๊ณผ 2๋ก ๋๋๋ค. `npm v5` ~ `npm v6`๋ 1์ด ์ฌ์ฉ๋๊ณ , `npm v7`์ด์์ 2๊ฐ ์ฌ์ฉ๋๋ค. (์ฌ๋ด์ผ๋ก `lockfileVersion` ๋ฒ์  3๋ ์๋๋ฐ, ์ด๋ `npm v7`์ด์์์ `node_modules/.package-lock.json`์ ์จ๊ฒจ์ง ์ ๊ธ ํ์ผ์ ์ฌ์ฉ๋๋ค. ๋ง์ฝ, ํ์ฌ ํจํค์ง๊ฐ `npm v6`์ ๋ํ ์ง์์ด ์๋ค๋ฉด ํฅํ ๋ฒ์ ์์ ์ฌ์ฉ ๋  ๊ฐ๋ฅ์ฑ์ด ๋๋ค.) ๊ฒฐ๋ก ์ ์ผ๋ก ๋ฌธ์  ์์ธ์ `lockfileVersion`์ด 2์์ 1๋ก ๋ฎ์์ง๋ฉด์ ์๊ธด ๋ฌธ์ ์ธ๋ฐ, ๋น์ ๋์ ๋ก์ปฌ ํ๊ฒฝ์ `npm` version์ `6.14.7`์ด์๊ณ , `node` version์ `14.8.0`์ด์๋ค. (`npm` ๋ฒ์ ์ ํ์ธํ๋ ค๋ฉด `npm -v`๋ฅผ, `node`์ ๋ฒ์ ์ ํ์ธํ๋ ค๋ฉด `node -v`๋ฅผ ์๋ ฅํ์.)

ํ์ง๋ง, ์ด์ ๊น์ง ๋๋ `npm`์ ๋ฒ์ ์ ๊ฐ์ ๋ก ๋ฎ์ถ์ ์ด ์์์๋๋ฐ ์ `downgrade`๊ฐ ๋๋์ง ๊ณฐ๊ณฐ์ด ์๊ฐํด๋ณด๋ `legacy` ํ๋ก์ ํธ๋ฅผ ์คํ์ํฌ ๋ ์ต์  `node` version ์ง์์ด ์๋์ด์ `nvm`์ผ๋ก `node` version์ ๊ฐ์ ๋ก ๋ฎ์ถ์๋๋ ํด๋น `node` version์ ํธํ๋๋ `npm` version์ผ๋ก ๋ฐ๋์๊ธฐ ๋๋ฌธ์ด๋ค. ์ดํ ์ฝ์งํด๋ณด๋ฉฐ ์๊ฒ๋ ์ฌ์ค์ธ๋ฐ `node-version`๊ณผ ํธํ๋๋ `npm` ๋ฒ์ ์ด ์์๋ค. (Reference 2๋ฒ ์ฐธ๊ณ )

๊ฒฐ๋ก ์ ์ผ๋ก <a href='https://nodejs.org/ko/'>nodejs.org</a>์ ์ ์ํ์ฌ node.js์์ ์ ๊ณตํ๋ ์์ ์ ์ธ ๋ธ๋ ๋ฒ์ ์ธ `16.15.1 LTS`๋ฅผ ์ค์นํ๊ณ , ํด๋น ๋ธ๋๋ฒ์ ๊ณผ ํธํ๋๋ `npm` ๋ฒ์ ์ธ `8.11.0`์ ์ค์นํ๊ณ ๋์ `npm install`์ ํ๋๊น `package-lock.json`์ `diff`๊ฐ ์์ด ์ ์์ ์ผ๋ก ์คํ๋์๋ค.

ํฐ๋ฏธ๋ ๋ช๋ น์ด๋ `nvm ls -remote`๋ก ์ง์๋๋ ๋ธ๋ ๋ฒ์ ์ ํ์ธํ๊ณ , `nvm install 16.15.1`๋ก ํด๋น ๋ธ๋ ๋ฒ์ ์ `nvm`์ ์ค์นํ๊ณ ,(`nvm`์ด ์๋ค๋ฉด ์ค์นํ๋ ๋ฐฉ๋ฒ์ ๊ตฌ๊ธ์ ์ฐพ์๋ณด์.) `npm use 16.15.1`๋ก ๋ธ๋ ๋ฒ์ ์ ์ ์ฉ์ํค๊ณ , ๋ง์ง๋ง `npm install -g npm@8.11.0`์ผ๋ก `npm`์ ๋ฒ์ ์ ๋ง์ถฐ์ฃผ์๋ค.

์ด๋ ๊ฒ ๋ ํ๋์ ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๋ค. ํน์๋ผ๋ ๋์ ๊ฐ์ ๋ฌธ์ ๋ก ๊ณ ์ํ๊ณ  ์๋ ๋ค๋ฅธ ๋ถ๋ค์๊ฒ ๋์์ด ๋์์ผ๋ฉด ์ข๊ฒ ๋ ์๊ฐ์ ํ๋ฉฐ ๊ธ์ ๋ง์น๋ค.

Reference
1. https://docs.npmjs.com/cli/v8/configuring-npm/package-lock-json
2. https://nodejs.org/ko/download/releases/
3. https://nodejs.org/ko/

---
## ๐ npm๊ณผ caret, tilde, ํจํค์ง ์ค์นํ๋ ๋ฐฉ๋ฒ ์์๋ณด๊ธฐ
`npm`์ ์๋ฐ์คํฌ๋ฆฝํธ ํจํค์ง ๋งค๋์ ๋ค. `Node.js`์์ ์ฌ์ฉํ  ์ ์๋ ๋ชจ๋๋ค์ ํจํค์งํํด์ ๋ชจ์๋ ์ ์ฅ์ ์ญํ ๊ณผ ํจํค์ง ์ค์น ๋ฐ ๊ด๋ฆฌ๋ฅผ ์ํ `CLI(Command line Interface)`๋ฅผ ์ ๊ณตํ๋ค. ์์ ์ด ์์ฑํ ํจํค์ง๋ฅผ ๊ณต๊ฐํ  ์๋ ์๊ณ , ํ์ํ ํจํค์ง๋ฅผ ๊ฒ์ํ์ฌ ์ฌ์ฌ์ฉํ  ์๋ ์๋ค.

๋ด๊ฐ ๋ค๋๋ ํ์ฌ์์๋ `npm` ํจํค์ง๋ฅผ ์ง์  ๋ง๋ค์ด ์ฌ์ฉํ๊ณ  ๊ด๋ฆฌํ๋ค.(<a href='https://www.npmjs.com/~triple-corp'>npm ํจํค์ง ๋ณด๋ฌ๊ฐ๊ธฐ</a>) ์ทจ์์ค๋นํ  ๋ ํ๋ก์ ํธ๋ฅผ ํ๋ฉด ํญ์ `ํจํค์ง === ๊ฐ์ ธ๋ค ์ฐ๋ ๊ฒ`์ด๋ผ๊ณ  ์๊ฐํ๋๋ฐ, ์ง์  ํจํค์ง๋ฅผ ๋ง๋ค๊ณ  ๋ฒ์ ์ ๊ด๋ฆฌํด์ผ ํ๋ค๋ณด๋ ๊น๋ค๋ก์ด ๊ฒ์ด ํ๋์ด ์๋์๋ค. ๊ทธ๋์ ํจํค์ง ๋ฒ์ ์ ๊ธฐ๋ณธ์ ์ธ ๋ด์ฉ๋ค์ ํ์์ ์ ์ฉํ  ๋ ๊น๋จน์ง ์๊ธฐ ์ํด ๊ธ์ ๋จ๊ธด๋ค. ์ฐ๋ฆฌ๊ฐ ํ๋ก์ ํธ๋ฅผ ๋ง์ง ๋ `package.json`์ ํจํค์ง๋ฅผ ์ค์นํ๋๋ฐ, ์ด๋ ํจํค์ง์๋ ๋ค์์ฒ๋ผ ๋ฒ์ ์ด ๋ช์๋์ด์๋ค.

```javascript
// package.json
"typescript": "3.8.3"
"lint-staged": "^8.1.1",
"nodemon": "~1.19.1",
```

์๋ฅผ ๋ค์ด, `typescript`์ ๊ฒฝ์ฐ์๋ `3.8.3`์ด๋ผ๊ณ  ๋ช์๋์ด์๋๋ฐ ์ด๋ฌํ ์ซ์๋ฅผ `semantic versioning`์ด๋ผ๊ณ  ๋ถ๋ฅธ๋ค. ๋ฒ์ ์ ์ผ์ชฝ๋ถํฐ ์ฐจ๋ก๋๋ก ์ฃผ(ไธป) ๋ฒ์ (Major), ๋ถ(้จ) ๋ฒ์ (Minor), ์(ไฟฎ) ๋ฒ์ (Patch)๋ผ๊ณ  ๋ถ๋ฅธ๋ค. `Major`๋ ๊ธฐ์กด ๋ฒ์ ๊ณผ ํธํ๋์ง ์๊ฒ API๊ฐ ๋ฐ๋๋ฉด ๋ณ๊ฒฝ๋๊ณ , ๊ธฐ์กด ๋ฒ์ ๊ณผ ํธํ๋๋ฉด์ ์๋ก์ด ๊ธฐ๋ฅ์ ์ถ๊ฐํ  ๋๋ `Minor`๋ฅผ ์ฌ๋ฆฌ๊ณ , ๊ธฐ์กด ๋ฒ์ ๊ณผ ํธํ๋๋ฉด์ ๋ฒ๊ทธ๋ฅผ ์์ ํ ๊ฒ์ด๋ผ๋ฉด `Patch`๋ฅผ ์ฌ๋ฆฐ๋ค. 

![](https://velog.velcdn.com/images/abcd8637/post/afbd85bf-3e75-42c9-bf81-9027b55147ee/image.jpeg)

์๋จ `codeBlock`์ ํจํค์ง ๋ฒ์ ์ ๋ณด๋ฉด ๋จ์ํ ์ซ์๋ง ๋ถ์ฌ์๋๊ฒ ์๋๋ผ ์ซ์ ์์ `^` ํน์ `~`๊ฐ ๋ถ์ด์๋๋ฐ, `^`๋ `caret(์บ๋ฟ)`์ด๋ผ ๋ถ๋ฅด๊ณ , ๋ฌผ๊ฒฐํ์์ธ `~`๋ `tilde(ํธ๋)`๋ผ๊ณ  ๋ถ๋ฅธ๋ค. ์ด๋ค์ ๊ฐ๊ฐ ํน์ง์ด ์๋๋ฐ `^`๋ `Compatible with version`์ ๋ปํ๋ฉฐ ๋์ค์ ์๋ฐ์ดํธ๋ฅผ ํ๋๋ผ๋ `major`๊ฐ์ด ๋ณํ์ง ์๋ ์ต๋ ๋ฒ์ ์ ์ค์นํ๊ฒ ๋๋ค. ์๋ฅผ ๋ค์ด `"lint-staged": "^8.1.1"` ํจํค์ง๋ฅผ ์๋ฐ์ดํธ ํ๊ฒ๋๋ฉด `major`๊ฐ์ด ๋ณํ์ง ์๋ ์ต์  ๋ฒ์ ์ธ `8.2.1`์ ๋ด๋ ค๋ฐ๊ฒ ๋๋ค. ์ด๋ฅผ ์ํด ๊ฐ๋จํ ์คํ์ ์งํํ๋๋ฐ, `npm init -y`๋ก `package.json`์ ๋ง๋ค๊ณ  `npm i lint-stages@8.1.1`๋ก `8.1.1`๋ฒ์ ์ ํจํค์ง๋ฅผ ๋ค์ด๋ฐ๊ณ ๋์ `npm i lint-stages`๋ฅผ ์งํํ๋ `8.2.1`๋ฒ์ ์ ๋ฐ์๋ค. ์ด๋ฅผ ํตํด ํจํค์ง๋ฅผ ์๋ฐ์ดํธํ๊ฒ ๋๋ฉด `^`์ ํด๋น `major`๋ฅผ ์ ์ธํ ๋ฒ์ ์ ์ต์  ๋ฒ์ ์ ๋ค์ด๋ฐ๋๋ค๋ ์ ์ ์ ์ ์์๋ค. 

![](https://velog.velcdn.com/images/abcd8637/post/a6ce9f9b-1d34-447e-8d93-b4d94f782e94/image.png)

![](https://velog.velcdn.com/images/abcd8637/post/b4241f5c-ac4f-4461-a5ac-e50aeeb48938/image.png)

๋ค์์ผ๋ก `~`๋ `tilde(ํธ๋)`๋ `Approximately equivalent to version`์ ๋ปํ๋ฉฐ, ํจํค์ง ๋ฒ์  ์ค์น ๋ช๋ น์ด๋ฅผ ์ด๋๊น์ง ์๋ ฅํ๋์ง์ ๋ฐ๋ผ ๋ค๋ฅด๋ค. ์๋ฅผ๋ค์ด `npm i nodemon@~1`์ด๋ผ๊ณ  ์์ฑํ๋ฉด ๋ฒ์ ์ `major`๊น์ง๋ง ๋ช์ํ๊ธฐ ๋๋ฌธ์ `minor`์ `patch` ๋ณ๊ฒฝ์ ํ์ฉํ๋ค. ๊ทธ๋์ ํ๋จ ์ฌ์ง์ฒ๋ผ `major`๋ 1์ด๊ณ  ๋๋จธ์ง๋ ์ต์ ๋ฒ์ ์ ๋ค์ด๋ฐ๋๋ค. ์ด๋ `1.x.x`์ ๊ฐ๋ค.

![](https://velog.velcdn.com/images/abcd8637/post/7a425b18-09b1-4cee-956d-4c873cdbe3bf/image.png)

๊ฐ์ ๋ฐฉ๋ฒ์ผ๋ก `npm i nodemon@~1.10`์ ๋ค์ด๋ฐ์ผ๋ฉด `patch`๋ณ๊ฒฝ์ ํ์ฉ ํ  ๊ฒ์ด๊ณ , ์ด๋ `1.10.x`์ ๊ฐ๋ค.

์ฌ๋ด์ผ๋ก `^` ํน์ `~`๋ฅผ ๋ถ์ด์ง ์๊ธฐ ์ํด `npm install nodemon@1.10.1`์ ์๋ ฅํ๊ณ  `package.json`์ ๋ณด๋ฉด ์๋์ผ๋ก caret์ด ๋ถ์ฌ์๋๋ฐ, ์ด๋ `npm save-prefix`๊ฐ ๊ธฐ๋ณธ์ ์ผ๋ก `^`์ผ๋ก ์ค์ ๋์ด์๊ธฐ ๋๋ฌธ์ด๋ค. ๋ง์ฝ, default๊ฐ์ `^` ๋์  `--save-exact`๋ `~`๋ก ๋ฐ๊พธ๊ณ  ์ถ๋ค๋ฉด `npm config set save-prefix '--save-exact'` ํน์ `npm config set save-prefix '~'`์ผ๋ก ๋ณ๊ฒฝํ๋ฉด ๋๋ค. ๋ณ๊ฒฝ๋ ๋ด์ฉ์ `npm config set ls -l` ๋ช๋ น์ด์์ `save-prefix` ํค ๊ฐ์์๋ ํ์ธ ํ  ์ ์๋ค. ๋ํ, `npm config`๋ฅผ ๊ฑด๋ค์ง์๊ณ  ๋ช๋ น์ด๋ฅผ ์ฌ์ฉํ์ฌ ์ ํํ ๋ฒ์ ์ ๋ฐ๊ณ  ์ถ๋ค๋ฉด `npm i --save-exact <package>@<version>`์ ์๋ ฅํ๋ฉด ๋๋ค.

Reference
1. https://poiemaweb.com/nodejs-npm
2. https://docs.npmjs.com/about-semantic-versioning
3. https://semver.org/lang/ko/
4. https://stackoverflow.com/questions/22343224/whats-the-difference-between-tilde-and-caret-in-package-json
5. https://bytearcher.com/goodies/semantic-versioning-cheatsheet/

---
## ๐ Call by Value, Call by reference, Call by sharing

CS ๊ณต๋ถ๋ฅผ ํ๋ฉฐ `Call by Value, Call by Reference`์ ๋ํด์ ๋ฐฐ์ฐ๊ณ  ์๋๋ฐ, ํท๊ฐ๋ฆฌ๋ ๊ธ๋ค์ด ๋ง์๋ค. ๊ฒฐ๋ก ์ ์ผ๋ก `JS`์์๋ `Call by reference, Call by sharing` ๋ ์ฉ์ด๋ณด๋ค `Call by Value` ์ฉ์ด๋ฅผ ์ฌ์ฉํ๋ ํธ์ด ์๋ง๋ค. ๋ค์ ์์๋ฅผ ๋ค์ด๋ณด์.

```javascript
// ์์๊ฐ ๋ณ๊ฒฝ์๋
function foo(argument){
    argument = 10;
    console.log(argument)    // 10
}

// ์ดํดํ๊ธฐ ์ฝ๊ฒ ์์ฑํ ํจ์
function foo(){
    let argument = 5;
    argument = 10;
    console.log(argument)    // 10
}

let argument = 5;
foo(argument);

console.log(argument);    // 5
```

์์๊ฐ(Primitive type: `string` `boolean` `number` `null` `undefined`)์ ์ฃผ์ ๋์  ๊ฐ ์์ฒด๋ฅผ ๋ณต์ฌํ๊ธฐ ๋๋ฌธ์ ํจ์๋ก ์ ๋ฌํ `argument`์ ์ค์ ๋ก ํจ์์์ ๋ฐ์ `argument`์ ๊ฐ์ ๋ค๋ฅธ ๊ฐ์ด ๋๋ค. ๋๋ฌธ์ ์ฒซ๋ฒ์งธ ํจ์ ๋ด๋ถ์ argument `console.log`๋ 10์ด๋๊ณ , ํจ์ ๋ฐ์์์ argument `console.log`๋ 5๊ฐ ๋๋ค. 

๋ค์์ผ๋ก ์์๊ฐ(Primitive type)์ด ์๋ ๊ฐ์ฒด(Object)์ ์์ฑ์ ๋ณ๊ฒฝํ๋ฉด ์ด๋ป๊ฒ ๋ ๊น?

```javascript
// ๊ฐ์ฒดํ์์ ์์ฑ ๋ณ๊ฒฝ
function foo(argument){
    argument.a = 5
    console.log(argument)    // { a : 5 }
}

// ์ดํดํ๊ธฐ ์ฝ๊ฒ ์์ฑํ ํจ์
function foo(){
    let arguments = argument
    argument.a = 10
    console.log(arguments)    // { a : 5 }
}

let argument = {a: 5} 
foo(argument)
console.log(argument)    // { a : 5 }
```

๊ฐ์ฒดํ์์ ์ธ์๋ ์ฃผ์๊ฐ์ ๊ฐ๋ ์๋ก์ด ๋ณ์๊ฐ ๋๋๋ฐ, ์ด๋๋ ๋์ผํ ๊ฐ์ฒด๋ฅผ ๋ฐ๋ผ๋ณด๊ณ ์๋ค ๋ฐ๋ผ์ ํจ์๋ด๋ถ์ `argument`๊ฐ ๋ฐ๋๋ฉด ํจ์ ์ธ๋ถ์ `argument`๋ ๊ฐ์ด ๋ฐ๋๋ค. ์ฆ ํจ์ ๋ด๋ถ์ ๊ฐ๊ณผ ์ธ๋ถ์ ๊ฐ์ด ํจ์ ๋ด๋ถ์ ๊ฐ์ผ๋ก ๋ฐ๋๋ค.

๊ทธ๋ผ ๋ง์ง๋ง์ผ๋ก ๋ค์์ ์ด๋ป๊ฒ ๋ ๊น?

```javascript
function foo(argument){
    argument = 5;
    console.log(argument)    // ?
}

let argument = {a: 10}
foo(argument)
console.log(argument)    // ?
```

์ฝ๋์ ํ๋ฆ์ ์ดํด๋ณด๋ฉด ์ด๋๋ ํจ์๋ก ๋์ด๊ฐ๋ `argument`๋ ๊ฐ์ฒด, ํจ์ ๋ด๋ถ์์ `argument`๋ ์์ ๊ฐ์ผ๋ก ๋ณ๊ฒฝํ๋ค. ์ด๋๋ ์์ฑ์ ์์ ํ๊ฒ์ด ์๋ ๊ฐ ์ ์ฒด๋ฅผ ๋ฐ๊พธ๋๊ฒ์ด๊ธฐ ๋๋ฌธ์ ํจ์๋ด๋ถ๋ `5`, ํจ์์ธ๋ถ๋ `{a : 10}`๋ก ๋์ค๊ฒ๋๋ค. ๋ง์ฝ, `c, c++`์ ๊ฐ์ด `call by reference`๊ฐ ์กด์ฌํ๋ ์ธ์ด๋ผ๋ฉด ๊ฐ์ด ํจ์ ์ธ๋ถ์ ๊ฐ๋ `5`๋ก ๋ฐ๋์์ ๊ฒ์ด๋ค. (๋์  `call by sharing`์ด๋ผ๊ณ ๋ ๋ถ๋ฆฌ๋๋ฐ ์ด๋ ์ ์ ๋ช์นญ์ด ์๋๋ผ๋ ๋ง๋ ์์ด ์ฌ์ฉํ์ง ์์๋ค.)

์ง๊ธ๊น์ง์ ๋ด์ฉ์ ํ ๋๋ก ์ ๋ฆฌํ๋ฉด `๊ฐ์ฒด์ ์์ฑ์ ์์  ํ ๋๋ ์ฐธ์กฐ ๊ด๊ณ์ง๋ง ๊ฐ ์์ฒด๋ฅผ ์์ ํ๋ ๊ฒฝ์ฐ ๊ทธ ๊ด๊ณ๊ฐ ๊นจ์ง๋ค`๋ผ๊ณ  ๊ฒฐ๋ก ์ ๋ด๋ฆด ์ ์๋ค. ๋, ํจ์ ๋ด์์ ์์๊ฐ์ ๋ณ๊ฒฝํ๋ ๊ฒฝ์ฐ ๊ฐ์ ๋ฐ๋์ง ์์ง๋ง, ํจ์ ๋ด์์ ๊ฐ์ฒด์ ์์ฑ์ ๋ณ๊ฒฝํ๋ ๊ฒฝ์ฐ ๊ฐ์ด ๋ฐ๋๋ค.

### ๐ reference
1. <a href='https://www.youtube.com/watch?v=-w-oJp6OVd4&t=9s'>ZeroCho์ JS ์ค๊ธ ๊ฐ์ข 12-1. call by value, call by reference, call by sharing</a>
2. <a href='https://developer.mozilla.org/ko/docs/Glossary/Primitive'>MDN - ์์ ๊ฐ</a>
3. <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Working_with_Objects'>MDN - Working_with_Objects</a>

---
## ๐ Spread ์ฐ์ฐ์(...)์ ํ์ฉ
`TS(TypeScript)`๋ฅผ ๊ณต๋ถํ๋ค๊ฐ `Spread`๋ฅผ ๋ค์ํ ๊ณณ์ ์ฌ์ฉํจ์ ๊นจ๋ซ๊ณ  ์ ๋ฆฌํ๊ธฐ ์ํด ๊ธ์ ๋จ๊ธด๋ค. ๋์ฒด๋ก ๋ค์๊ณผ ๊ฐ์ ์ํฉ์์ ์์ฃผ ์ฐ์ธ๋ค.

1. ๊ฐ์ฒด ๋ณต์ฌ(์ฃผ์ ์ฐธ์กฐ๊ฐ ์๋ ๊ฐ ๋ณต์ฌ, `origin` ๊ฐ ๋ณ๊ฒฝ X)
2. ๊ฐ์ฒด ๋ณํฉ
3. ๊ฐ์ฒด ํน์  ๊ฐ ๋ณ๊ฒฝ(`useState` ์ฌ์ฉ ์)
4. ์์ฌ ์ฐ์ฐ์(rest operator) ์ฌ์ฉ

๋ค์์ ์ฝ๋๋ฅผ ํ๋์ฉ ์ดํด๋ณด์.

```javascript
// 1. ๊ฐ์ฒด ๋ณต์ฌ

// * spread ์ฐ์ฐ์ ๋ฏธ ์ฌ์ฉ(์ฃผ์ ์ฐธ์กฐ, ๋ณธ๋์ ๊ฐ์ด ๋ฐ๋๋ค.)
const myScore = [80, 85, 90];
const newScore = myScore
newScore[0] = 50
console.log(myScore)
๐๐ฝ [50, 85, 90]

// * spread ์ฐ์ฐ์ ์ฌ์ฉ(๊ฐ ์ฐธ์กฐ, ๋ณธ๋์ ๊ฐ์ด ๋ฐ๋์ง ์๋๋ค.)
const myScore = [80, 85, 90];
const newScore = [...myScore];
newScore[0] = 50
console.log(myScore)
๐๐ฝ [80, 85, 90]

// * ๊ฐ์ฒด ๋ณต์ฌ
const myInfo = {name: 'AYW', age:27,  marriage: false}
const newInfo = {...myInfo}
console.log(newInfo)
๐๐ฝ {name: 'AYW', age:27,  marriage: false}

// 2. ๊ฐ์ฒด ๋ณํฉ
const firstInfo = {name: 'AYW'}
const secondInfo = {age: 27}
const resultInfo = {...firstInfo, ...secondInfo}
console.log(resultInfo)
๐๐ฝ {name: 'AYW', age:27}

// 3. ๊ฐ์ฒด ํน์  ๊ฐ ๋ณ๊ฒฝ
const myInfo = {name: 'AYW', age:27,  marriage: false}
const newInfo = {...myInfo, marriage: !myInfo.marriage}
console.log(newInfo)
๐๐ฝ {name: 'AYW', age:27,  marriage: true}

// 4. ์์ฌ ์ฐ์ฐ์(spread operator) ์ฌ์ฉ
const myInfos = {money: 3000, name: 'AYW', age:27, }
const {money, ...restInfo} = myInfos
console.log(restInfo)
๐๐ฝ {name: 'AYW', age:27}
```

---
## ๐ Class vs ProtoType
`JavaScript`๋ `๊ฐ์ฒด์งํฅ์ธ์ด`์ด๊ธด ํ์ง๋ง `Class` ๋ฌธ๋ฒ์ ์ฌ์ฉํ์ง ์๊ณ  ๋์  `Prototype` ๋ฌธ๋ฒ์ ์ฌ์ฉํ๋ค. (ES6 ์ดํ์ `class` ๋ฌธ๋ฒ์ด ๋ค์ด์์ง๋ง ์ด๋ `JS`๊ฐ `class` ๊ธฐ๋ฐ์ ๋ฌธ๋ฒ์ผ๋ก ๋ฐ๋๊ฒ์ ์๋๋ค.) ๊ทธ๋ ๋ค๋ฉด `class`์ฒ๋ผ ์ฌ์ฉํ๋ ๋ฌธ๋ฒ๊ณผ `prototype` ๋ฌธ๋ฒ์ ์ธ์  ์ฌ์ฉํ ๊น? ๋ค์ ์ฝ๋๋ฅผ ์ดํด๋ณด์.

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

์ฐจ์ด์ ์ด ๋๊ปด์ง๋๊ฐ? `class` ๋ฌธ๋ฒ์ ๋จ์ ์ `myInfo` ๋ด์ ์๋ ๊ฐ ๋ชจ๋๊ฐ ๋ฉ๋ชจ๋ฆฌ์ ํ ๋น๋๋ค๋ ๊ฒ์ด๋ค. ์ฆ, `100๊ฐ`์ ๊ฐ์ฒด๋ฅผ ์ ์ธํ๋ฉด `200๊ฐ`์ ๊ฐ์ด ๋ฉ๋ชจ๋ฆฌ ๋ณ์์ ์ฌ๋ผ๊ฐ์๋ค๋ ๊ฒ์ด๋ค. ๋ง์ฝ, `a` ๋ณ์์๋ `name`๊ณผ `age` ๋ชจ๋๋ฅผ ์ฌ์ฉํ์ง๋ง, `b` ๋ณ์์๋ `name`๋ง ์ฌ์ฉํ๊ณ  ์ถ์ง๋ง ์ฌ์ฉํ์ง ์๋ `age` ๋ณ์๊น์ง ๋ฉ๋ชจ๋ฆฌ์ ํ ๋น๋๋ค๋ ์ด์ผ๊ธฐ๋ค. ์ด๋ ๊ณง ๋ฉ๋ชจ๋ฆฌ์ ํจ์จ๊ณผ ๊ด๋ จ์ด ๋๋๋ฐ ์ด๋ฅผ ๋ฐฉ์งํ๊ธฐ ์ํด `prototype`์ ์ฌ์ฉํ๋ค. 

`prototype`์ ํจ์๋ฅผ ์ง์ ํ  ๋ `prototype`์ด๋ผ๋ ๊ณ ์  ์์ฑ ๊ฐ์ ์ง์ ํด์ค์ผ๋ก์ ๊ฐ์ฒด๋ฅผ ์์ฑํ  ๋๋ง๋ค ํด๋น ๊ฐ์ฒด๋ ์ด๋ฏธ ์ง์ ๋ `prototype`์ ์ํ ํจ์ ๊ฐ๋ง ์ฐธ์กฐํ๋ฉด ๋๋๊ฒ์ด๋ค. ์ฆ, `prototype`์ ์ฐ๋ฉด 200๊ฐ์ ๋ณ์๊ฐ ๋ฉ๋ชจ๋ฆฌ์ ์ ์ฅ๋๋ ๊ฒ์ด์๋๋ผ, ํจ์ ๊ฐ๋ง ์ฐธ์กฐํ๋ฏ๋ก ๋ฉ๋ชจ๋ฆฌ๊ฐ ์ ์ฝ๋๋ ํจ๊ณผ๋ฅผ ์ผ์ผํฌ ์ ์๋ค.

`prototype` ์ฝ๋์ ํ๋ฆ์ ๋ค์๊ณผ ๊ฐ๋ค.
1. `prototype`์ ๋น ๊ฐ์ฒด๋ฅผ ์ด๋๊ฐ์ ์ ์ธํ๋ค.
2. `myName`๊ณผ `myAge`๋ `prototype`์ ๋น ๊ฐ์ฒด ๋ด์ ์กด์ฌํ๋ `Object`์ ๊ฐ์ ๋ชจ๋ ์ฌ์ฉํ  ์ ์๋ค.
  * ์ฝ๊ฒ ๋งํด `name`๊ณผ `age`๋ฅผ `prototype`์ ๋น ๊ฐ์ฒด ์ด๋๊ฐ์ ๋ฃ์ด๋๊ณ  `myName`๊ณผ `myAge` ๋ณ์๊ฐ ํด๋น ๊ฐ์ ๊บผ๋ด์ฐ๋ ํํ๋ผ๊ณ  ๋ณด๋ฉด ๋๋ค.

์ง๊ธ๊น์ง ๋ด์ฉ์ผ๋ก ๋ณด์ `prototype`์ ์ง์  ํจ์๋ด๋ถ์ ์ ์ธํ์ง๋ ์์ ๊ฐ์ ๋ฃ์ด๋ ์ ์๋ค๊ณ  ๋ฐฐ์ ๋ค. ๊ทธ๋ผ ์ด๋ป๊ฒ ๊ฐ์ ์ ์ฅํ๊ณ  ์ ๊ทผํ๋ค๋ ๋ง์ธ๊ฐ? ํด๋ต์ `prototype Object` ์ `prototype Link`์ ์๋๋ฐ, ํ๋์ฉ ์ฐจ๊ทผ์ฐจ๊ทผ ์ดํด๋ณด์. 

### ๐ ProtoType Object(constructor, __proto__)
๊ฐ์ฒด๋ ์ธ์ ๋ `ํจ์(Function)`๋ก ์์ฑ๋๋ค. ํจ์๋ก ์ ์๋๋ฉด ๊ธฐ๋ณธ์ ์ผ๋ก ์ป๋ ์๊ฒฉ์ด ์๋๋ฐ ๋ฐ๋ก `constructor(์์ฑ์)`๋ค. ์ด๊ฒ์ด ์๊ธฐ ๋๋ฌธ์ ์ฐ๋ฆฌ๋ `new`๋ฅผ ํตํด ๊ฐ์ฒด๋ฅผ ๋ง๋ค ์ ์๋ ๊ฒ์ด๋ค. ๋, ํจ์๋ฅผ ์์ฑํ๋ฉด `prototype`์ ์์ฑ๊ณผ `ProtoType Object`๊ฐ ๋ง๋ค์ด์ง๋๋ฐ, ์์ฑ๋ ํจ์์ `prototype`์ ์์ฑ์ ํตํด `ProtoType Object`์ ์ ๊ทผํ  ์ ์๋ ๊ถํ์ด ์๊ธด๋ค. `ProtoType Object`๋ ๋ง ๊ทธ๋๋ก ํ๋กํ ํ์ ๊ฐ์ฒด์ด๊ณ , ์ดํดํ๊ธฐ ์ฝ๊ฒ ์ผ๋ฐ ๊ฐ์ฒด๋ผ๊ณ  ์๊ฐํ๋ฉด๋๋ค. ์ผ๋ฐ ๊ฐ์ฒด์๋ ๊ฐ์ ์์ ๋กญ๊ฒ ์ถ๊ฐํ๊ฑฐ๋ ์ญ์  ํ  ์ ์๋ค. 

![](https://images.velog.io/images/abcd8637/post/cf3b7e9d-5255-4fea-a8f7-5df51d76597a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-15%2006.36.35.png)

![](https://images.velog.io/images/abcd8637/post/35babb3d-367a-4634-aa1f-30e1840384f9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-15%2006.39.12.png)

`prototype_object`๋ `constructor(์์ฑ์)`์ `__proto__` ์์ฑ์ ๊ฐ๊ฒ ๋๋ค. `constructor(์์ฑ์)`๋ ์์ฑ ํจ์์ธ `function myInfo() {}`๋ฅผ ๊ฐ๋ฆฌํค๊ณ  ์๋ค. ๊ทธ๋ ๋ค๋ฉด `__proto__`๋ ๋ญ๊น? ์ฐ๋ฆฌ๊ฐ ํจ์ ๋ด๋ถ์ ๊ฐ์ ์ ์ธํ์ง ์์๋๋ฐ๋ ์ ๊ทผํ  ์ ์๋ ์ด์ ๊ฐ ๋ฐ๋ก `__proto__`๋๋ฌธ์ด๋ค. `__proto__`๋ ๊ฐ์ฒด๊ฐ ์์ฑ๋  ๋ `origin` ๊ฒฉ์ ํจ์์ธ `prototype object`๋ฅผ ๊ฐ๋ฆฌํจ๋ค. ์ฆ, `name`๊ณผ `age`๋ `myInfo`๋ก๋ถํฐ ์์ฑ๋์์ผ๋ `myInfo` ํจ์์ `prototype object`๋ฅผ ๊ฐ๋ฆฌํค๊ณ  ์๋๋ค. ์ด์ฒ๋ผ `__proto__` ์์ฑ์ ํตํด ์์ ํ๋กํ ํ์๊ณผ ์ฐ๊ฒฐ๋์ด์๋ ํํ๋ฅผ `ํ๋กํ ํ์ ์ฒด์ธ(prototype chain)`์ด๋ผ๊ณ  ํ๋ค.

```javascript
function myInfo() {}    // ํจ์

myInfo.prototype.name = 'AYW'
myInfo.prototype.age = 27

let an = new myInfo();  // ํจ์๋ฅผ ๊ฐ์ฒด๋ก ์์ฑ

console.log(an.name) // 'AYW'
console.log(an.age)  // 27
```

![](https://images.velog.io/images/abcd8637/post/e66d7342-83ec-43bf-ae82-52e67dac16c2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-15%2006.43.09.png)

`prototype chain`์ ๋ ๋ค๋ฅธ ์๋ฅผ ํ๋ฒ ์ดํด๋ณด์.

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

์ด ๊ณผ์ ์ `Boss` -> `MiddleBoss` -> `Worker` ์์ผ๋ก `boolean`๊ฐ์ ๋ด๋ ค์ฃผ๋ ์ฝ๋์ด๋ค. ์ด ์ฝ๋๊ฐ ์ด๋ป๊ฒ `o.boolean`๊ฐ์ด `true`๊ฐ ๋์ฌ ์ ์์๊น?

1. ๋ง์ง๋ง ๋ณ์์ธ `o`์ `property` ์ค `boolean` ๊ฐ์ด ์๋์ง ์ดํด๋ณด๊ณ  ์์ผ๋ฉด ๋ฐํ์ํจ๋ค.
2. ๊ฐ์ด ์์ผ๋ฉด ์์ฑ์๋ฅผ ์ถ์ ํ๋ค. ์์ `prototype`์ธ `Worker`์ `property`์์ `Worker.prototype.boolean`์ ์ฐพ๊ณ  ์์ผ๋ฉด ๋ฐํ์ํจ๋ค.
3. ๊ฐ์ด ์์ผ๋ฉด ์์ฑ์๋ฅผ ์ถ์ ํ๋ค. ์์ `prototype`์ธ `MiddleBoss`์ `property`์์ `MiddleBoss.prototype.boolean` ์ฐพ๊ณ  ์์ผ๋ฉด ๋ฐํ์ํจ๋ค.
4. ๊ฐ์ด ์์ผ๋ฉด ์์ฑ์๋ฅผ ์ถ์ ํ๋ค. ์์ `prototype`์ธ `Boss`์ `property`์์ `Boss.prototype.boolean` ์ฐพ๊ณ  ์์ผ๋ฉด ๋ฐํ์ํจ๋ค.

์ด๋ฐ ์์๋ก ์งํ ๋  ๊ฒ์ด๋ค. ๋ง์ฝ, ๋ค์ ์ฝ๋๋ ๊ฐ์ด ์ด๋ป๊ฒ ๋์ฌ๊น?

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

์ ๋ต์ `false`๋ค. ์์๋ ๋ค์๊ณผ ๊ฐ๋ค.

1. ๋ง์ง๋ง ๋ณ์์ธ `o`์ `property` ์ค `boolean` ๊ฐ์ด ์๋์ง ์ดํด๋ณด๊ณ  ์์ผ๋ฉด ๋ฐํ์ํจ๋ค.
2. ๊ฐ์ด ์์ผ๋ฉด ์์ฑ์๋ฅผ ์ถ์ ํ๋ค. ์์ `Worker.prototype`๋ฅผ ์ดํ๋ค.
3. `prototype`์ ์ ์ฅ๋์ด์๋ `boolean` ๊ฐ์ด `4`์ด๊ธฐ ๋๋ฌธ์ `4`๋ฅผ ๋ฐํํ๋ค.

reference
1. <a href='https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67'>javascript-prototype-์ดํดํ๊ธฐ</a>
2. <a href='http://insanehong.kr/post/javascript-prototype/'>Javascript ๊ธฐ์ด - Object prototype ์ดํดํ๊ธฐ</a>
3. <a href='https://opentutorials.org/module/4047/24610'>์ํ์ฝ๋ฉ - prototype</a>

---
### ๐ ๋ฐฐ์ด์ ํน์  ์ธ๋ฑ์ค๋ฅผ ์ ๊ฑฐํ๊ฑฐ๋ ํน์  ๋ฒ์๋ง ๋ฐํํ๋ ํจ์, filter, splice, slice

์๊ณ ๋ฆฌ์ฆ ๋ฌธ์ ๋ฅผ ํ๋ค๊ฐ ๋ฐฐ์ด์ ํน์ ์์๋ ์ธ๋ฑ์ค๋ฅผ ์ญ์ ํ๊ฑฐ๋ ํน์  ๋ฒ์๋ฅผ ์ ๊ฑฐํ๊ณ  ์ถ์๋ฐ `MDN`์ ์ฐพ์๋ณด๋๊น ์ฌ๋ฌ๊ฐ์ง ํจ์๊ฐ ์กด์ฌํ๋ค. ์ํฉ๋ง๋ค ๋ค๋ฅด์ง๋ง ๋ก์ง์ ๊ตฌํ ํ  ๋ ๋ค์์ ์ ํ์ง๋ฅผ ๋ณด๊ณ  ๊ณจ๋ผ์ ์ฌ์ฉํ๋ฉด ๋๋ค.

์๋ณธ ๋ฐฐ์ด์ ๋ณ๊ฒฝํ๊ณ  ์ถ์ง์๊ณ  ํน์  ์ธ๋ฑ์ค๋ฅผ ์ ๊ฑฐํ๊ณ  ์ถ์ ๋(์๋ก์ด ๋ณ์์ ์ ์ธํ๊ณ  ์ถ์ ๋)
1. <a href='https://developer.mozilla.org/ko/docs/orphaned/Web/JavaScript/Reference/Global_Objects/Array/filter'>filter</a>: ์ฃผ์ด์ง ํจ์์ ํ์คํธ๋ฅผ ํต๊ณผํ๋ ๋ชจ๋  ์์๋ฅผ ๋ชจ์ ์๋ก์ด ๋ฐฐ์ด๋ก ๋ฐํํ๋ค. `arr.filter(callback(element[, index ?[, array ?]])[, thisArg ?])`

์๋ณธ ๋ฐฐ์ด์ ๋ณ๊ฒฝํ๊ณ  ํน์  ์ธ๋ฑ์ค๋ฅผ ์ ๊ฑฐํ๊ณ  ์ถ์ ๋(์๋ก์ด ๋ณ์์ ์ ์ธํ๊ณ  ์ถ์ ์์ ๋)
1. <a href='https://developer.mozilla.org/ko/docs/orphaned/Web/JavaScript/Reference/Global_Objects/Array/splice'>splice</a>: ๋ฐฐ์ด์ ๊ธฐ์กด ์์๋ฅผ ์ญ์  ๋๋ ๊ต์ฒดํ๊ฑฐ๋ ์ ์์๋ฅผ ์ถ๊ฐํ์ฌ ๋ฐฐ์ด์ ๋ด์ฉ์ ๋ณ๊ฒฝํ๋ค. `(array.splice(start, delete ?, item ?))`

์๋ณธ ๋ฐฐ์ด์ ๋ณ๊ฒฝํ๊ณ  ์ถ์ง์๊ณ  ํน์  ๋ฒ์๋ฅผ ์๋ผ๋ด๊ณ  ์ถ์ ๋
1. <a href='https://developer.mozilla.org/ko/docs/orphaned/Web/JavaScript/Reference/Global_Objects/Array/slice'>slice</a>: ์ด๋ค ๋ฐฐ์ด์ `begin`๋ถํฐ `end`๊น์ง(`end`๋ ๋ฏธํฌํจ)์ ๋ํ ์์ ๋ณต์ฌ(shallow copy)๋ณธ์ ์๋ก์ด ๋ฐฐ์ด ๊ฐ์ฒด๋ก ๋ฐํ์ํจ๋ค. `arr.slice(begin ?, end ?)`

์ ์๋ง ๋ด์๋ ์ ์ดํด๊ฐ ์๋๋๊น ์ฝ๊ฒ ์๋ฅผ ๋ค์ด๋ณด์. ๋ค์์ `arr`์์ ๋๋ `arr[2]`์ ๊ฐ์ ์ ๊ฑฐํ๊ณ , `arr`์ `[1 : 3]`๋ฒ์๋ง ์๋ฅด๊ณ  ์ถ๋ค.

```javascript
const arr = [0, 2, 1, 6]

// filter
const filterArr = arr.filter((item) => item !== arr[2])
console.log(filterArr)
๐๐ฝ [0, 2, 6]

// splice
arr.splice(2)
console.log(arr)
๐๐ฝ [0, 2, 6]

// slice
const sliceArr = arr.slice(1, 3)
console.log(sliceArr)
๐๐ฝ [2, 1]
```

์ฌ๋ด์ผ๋ก `slice`๋ ์๋ณธ ๊ฐ์ฒด๋ฅผ `์์ ๋ณต์ฌ(shallowCopy)`๊น์ง ๊ฐ๋ฅํ๋ฐ, ์ ๋ง `์์ ๋ณต์ฌ(shallowCopy)` ๊น์ง๋ง ๋๋์ง ์คํํด๋ดค๋ค. ์คํ๊ฒฐ๊ณผ `depth = 2`์ธ ๊ฐ์ ๋ณ๊ฒฝ์ ์๋ณธ๊ฐ๋ ๋ณ๊ฒฝ๋๊ฒ์ผ๋ก ๋ณด์ ์์๋ณต์ฌ๊น์ง๋ง ๊ฐ๋ฅํ๋ค.

```javascript
// depth = 1์ผ ๋ slice
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
const copyArr = arr.slice();
arr[2] = -9999

console.log(copyArr)
๐๐ฝ [1, 2, 3, 4, 5, 6, 7, 8, 9];

// depth = 2์ผ ๋ slice
const arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];
const copyArr = arr.slice();
arr[0][2] = -9999

console.log(copyArr)
๐๐ฝ [[1, 2, -9999], [4, 5, 6], [7, 8, 9]];
```

---
### ๐ ์ผ๊ธ๊ฐ์ฒด(First Class Object)์ ๋ํด์ ๊ฐ๋ตํ๊ฒ ์์๋ณด์
ํ๋ก ํธ์๋ ์ ์ ๋ฉด์ ์์ `First Class Object`๊ฐ ๋ฌด์์ธ์ง ์ค๋ชํด์ฃผ์ธ์. ๋ผ๋ ์ฒซ ์ง๋ฌธ์ ๋ฃ๊ณ  ๋จธ๋ฆฌ๊ฐ ๋ฒ์ช๋ค. ์ฉ์ด๋ฅผ ๋ค์ด๋ดค์ผ๋ฉด ๊ด๋ จ๋ ๋ด์ฉ์ ์ฅ์ด์ง๋ด๊ธฐ๋ผ๋ ํ  ํ๋ฐ ๋๋ฌด์ง ์๊ฐ์ด ๋์ง ์์๊ธฐ ๋๋ฌธ์ด๋ค.

![](https://images.velog.io/images/abcd8637/post/b1f82ec5-8b0b-43dc-85ee-c70e3ceeb1a5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-09-03%2006.48.36.png)

๋ต๋ตํจ์ ์ด๊ธฐ์ง ๋ชปํ๊ณ  ๋ฉด์ ๊ด๋ถ๊ป ์์ํ๊ฒ ๋ง์๋๋ ธ๋ค. ๊ทธ๋ฐ ์ฉ์ด๋ฅผ ์ฒ์ ๋ฃ์ต๋๋ค.. ๊ณต๋ถ๋ฅผ ๋ ํด์ผ๊ฒ ์ต๋๋ค.. ๋ฉด์ ๊ด๋์ ์ผ๊ธ ๊ฐ์ฒด์ ๋ํด ์์ธํ ์ค๋ช์ ํด์ฃผ์จ๋ค. ๊ทธ๋ฆฌ๊ณ  ๋ฐ๋ ค์ค๋ ์๊ดด๊ฐ..

![](https://images.velog.io/images/abcd8637/post/dbfc2358-95f7-45f3-a300-66ca7d904472/More_details_be_omitted.jpeg)

๋น๋ก ๋ฉด์ ์ ๋จ์ด์ง ์ด์ ๊ฐ ์ผ๊ธ๊ฐ์ฒด๋ฅผ ๋ชฐ๋ผ์ ๋จ์ด์ง๊ฒ์ ์๋์ง๋ง `JS`์ ๊ธฐ์ด์ ์ธ ๋ด์ฉ์ ๋ง์ด ๊ณต๋ถํด์ผ๊ฒ ๋ค๋ ์๊ฐ์ด ๋ค์๋ค. ๊ทธ๋์ ์์๋ณด์ ์ผ๊ธ๊ฐ์ฒด(`First class object`)๋ ๋ฌด์์ธ๊ฐ?

`MDN`์์ ์ฐพ์๋ณด๋๊น ๋ค์๊ณผ ๊ฐ์ ์ ์๊ฐ ๋์จ๋ค. `ํจ์`๋ฅผ ๋ณ์์ฒ๋ผ ๋ค๋ฃจ๋ ์ธ์ด๋ ์ผ๊ธ ํจ์๋ฅผ ๊ฐ์ก๋ค๊ณ  ํํํ๋ค. ์ด ๋ฌธ์ฅ์ ๋ณด๊ณ  ๋ฐ๋ก `ํจ์ ํํ์`์ด ์๊ฐ๋ฌ๋ค. ํจ์ํํ์๊ณผ ํจ์์ ์ธ์์ ์ฐจ์ด๋ ์๋ค๊ณ  ์ฐ์ญ๋์ง๋ง ์ ์ `์ผ๊ธ๊ฐ์ฒด`๋ฅผ ๋ชจ๋ฅด๋ ๋.. 

![](https://images.velog.io/images/abcd8637/post/7d52d695-9b60-4ee8-a90d-4b2f3fb18cb2/jjvNX.jpeg)

์ด์ธ์๋ ๋ค์๊ณผ ๊ฐ์ ๊ฒฝ์ฐ๋ฅผ ๋ง์กฑํ๋ฉด ์ผ๊ธ ๊ฐ์ฒด๋ผ๊ณ  ๋ถ๋ฅธ๋ค. ์ฝ๋์ ํจ๊ป ์ดํด๋ณด์.

1. Functions can be assigned to variables: ํจ์๋ฅผ ๋ณ์์ฒ๋ผ ์ ์ธ ํ  ์ ์๋๊ฐ?

```javascript
const sayHi = function(){
    return "Hi";
}

console.log(sayHi());
๐๐ฝ Hi
```

2. Functions can be passed to arguments to other functions: ํจ์์ ์ธ์๋ก ํจ์๋ฅผ ๋ฃ์ ์ ์๋๊ฐ?
```javascript
const sayHelloToPerson = (greet, person) => {
    return greet() + " " + person
}

const sayGreet = function(){
    return "Hello,"
}

console.log(sayHelloToPerson(sayGreet, "Ted"));
๐๐ฝ Hello, Ted
```

3. Functions can be returned from other functions: ํจ์์ ๋ฆฌํด ๊ฐ์ผ๋ก ํจ์๋ฅผ ์ฌ์ฉ ํ  ์ ์๋๊ฐ?
```javascript
const sayHello = function(){
    return function(greet){
        return greet
    }
}

const sayHelloOuter = sayHello();
const sayHelloInner = sayHelloOuter("Hello");
console.log(sayHelloInner)
๐๐ฝ Hello

// closure๋ฅผ ์ฌ์ฉํ ๋ฐฉ๋ฒ
const sayYouAndMe = function(yourName){
    return function(myName){
        return yourName + "๊ณผ " + myName;
    }
}

const sayYourName = sayYouAndMe("elon");
const sayMyName = sayYourName("Ted");
console.log(sayMyName);
๐๐ฝ elon๊ณผ Ted
```

reference
1. <a href ='https://developer.mozilla.org/ko/docs/Glossary/First-class_Function'>MDN</a>
2. <a href='https://www.youtube.com/watch?v=4UeWzn4jzwM'>First Class Functions in JavaScript - Youtube</a>

---
### ๐ ๋ฉ์๋ ๋ด๋ถ ํจ์์์ this๋ฅผ ์ฐํํ๋ ๋ฐฉ๋ฒ 4๊ฐ์ง
๋ง์ฝ, `scope chain`์ฒ๋ผ ๋ณ์๊ฐ ์์ ๋ ๊ฐ์ฅ ๊ฐ๊น์ด ์ค์ฝํ์ `Lexical Environment`๋ฅผ ์ฐพ๊ณ , ๊ฑฐ๊ธฐ๋ ์์ผ๋ฉด ์์ ์ค์ฝํ๋ฅผ ํ์ํ๋ฏ์ด `this`๋ ํธ์ถ ์ฃผ์ฒด๊ฐ ์์ ๋๋ ์๋์ผ๋ก ์ ์ญ๊ฐ์ฒด๋ฅผ ๋ฐ์ธ๋ฉํ์ง ์๊ณ  ํธ์ถ ๋น์ ์ฃผ๋ณ ํ๊ฒฝ์ `this`๋ฅผ ์์ํ๊ณ  ์ถ๋ค๋ฉด ์ด๋ค ๋ฐฉ๋ฒ์ ์ฌ์ฉํ ๊น? ํ์ฌ ์ปจํ์คํธ์ ๋ฐ์ธ๋ฉ๋ ๋์์ด ์์ผ๋ฉด ์ง์  ์ปจํ์คํธ์ `this`๋ฅผ ๋ฐ๋ผ๋ณด๋ ๊ฒ์ฒ๋ผ ๋ง์ด๋ค. ์์ฝ๊ฒ๋ `ES5` ๊น์ง๋ ์์ฒด์ ์ผ๋ก ๋ด๋ถํจ์์ `this`๋ฅผ ์์ํ  ๋ฐฉ๋ฒ์ ์์ง๋ง ์ฐํํ๋ ๋ฐฉ๋ฒ์ ์๋ค. ์ง๊ธ๋ถํฐ ๋ฉ์๋์ ๋ด๋ถํจ์์์ ๋ฉ์๋์ `this`๋ฅผ ๊ทธ๋๋ก ๋ฐ๋ผ๋ณด๊ฒ ํ๊ธฐ ์ํ ๋ฐฉ๋ฒ 4๊ฐ์ง๋ฅผ ์์๋ณด์. ๊ทธ๋ฆฌ๊ณ  `ES6`์์ `this`๋ฅผ ๋ฐ์ธ๋ฉํ์ง ์๊ณ  ์์ ์ค์ฝํ์ `this`๋ฅผ ๊ทธ๋๋ก ํ์ฉ๊ฐ๋ฅํ `ํ์ดํ ํจ์(arrow-function)`๋ก๋ ์ฌ์ฉํ ์ฝ๋๋ฅผ ์ดํด๋ณด์. ๊ฒฐ๊ณผ๋ ๋ชจ๋ ๋์ผํ๋ค.

1. `this` ๋ณ์ ์ ์ฅ
2. `call`
3. `bind`
4. `ํ์ดํํจ์`

```javascript
// this ๋ณ์ ์ ์ฅ
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


๐๐ฝ { outer: [Function: outer] }
๐๐ฝ { outer: [Function: outer] }
```

reference
1. <a href='https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=206513031'>์ฝ์ด ์๋ฐ์คํฌ๋ฆฝํธ - ์ ์ฌ๋จ</a>

---
### ๐ 2์ฐจ์ ๋ฐฐ์ด ์ ์ธํ๊ธฐ
`Array.from` ๋ฉ์๋๋ฅผ ์ฌ์ฉํ์ฌ 2์ฐจ์ ๋ฐฐ์ด์ ์ ์ธ ํ  ์ ์๋ค. ์๋ฅผ ๋ค์ด 3ํ 4์ด์ 0์ผ๋ก ์ด๊ธฐํํ 2์ฐจ์ ๋ฐฐ์ด์ ๋ง๋ค๊ณ  ์ถ๋ค๋ฉด ๋ค์๊ณผ ๊ฐ์ด ์์ฑํ๋ค.

```javascript
let row = 3;
let column = 4;
let arr = Array.from(Array(row), ()=> Array(column).fill(0));

console.log(arr)
๐๐ฝ [ [ 0, 0, 0, 0 ], [ 0, 0, 0, 0 ], [ 0, 0, 0, 0 ] ]
```

reference
1. <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from'>MDN</a>

---
### ๐ DOM ์์ฑ์ ์ด๋ฒคํธ ํธ๋ค๋ฌ ์ฐ๊ฒฐํ๊ธฐ
์ด๋ฒคํธ ํธ๋ค๋ฌ๋ฅผ DOM์์์ ์ฐ๊ฒฐ ํ  ๋ ๋ณดํต 2๊ฐ์ง ๋ฐฉ๋ฒ์ ์ฌ์ฉํ๋๋ฐ, ์ฒซ๋ฒ์งธ๋ก `on`์์ฑ๊ณผ, ๋๋ฒ์งธ๋ `addEventListener`์ด๋ค. ์ ์์ ๊ฒฝ์ฐ ๋น ๋ฅด์ง๋ง ์ง์ ๋ถํ ๋ฐฉ๋ฒ์ด๋ผ๊ณ  ๋งํ๋ค. ์๋ฅผ ๋ค์ด `ondblclick`, `onmouseover`, `onblur`, `onfocus` ์์ฑ๋ ์๋ค.

```javascript
const button = document.querySelector("button");
button.onclick = () => {
    console.log("Click managed using onclick property");
}
```

![](https://images.velog.io/images/abcd8637/post/5b42d540-0dce-47f7-adb6-51e47cbbf645/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-21%2021.46.50.png)

์ด๋ฐ ํด๊ฒฐ์ฑ์ ์ ๋์ํ๋๋ผ๋ ์ผ๋ฐ์ ์ผ๋ก ๋์ ๊ดํ์ผ๋ก ๊ด์ฃผ๋๋๋ฐ, ๊ทธ ์ด์ ๋ ์์ฑ์ ์ฌ์ฉํ๋ฉด ํ๋ฒ์ ํ๋์ ํธ๋ค๋ฌ๋ง ์ฐ๊ฒฐํ  ์ ์๊ธฐ ๋๋ฌธ์ด๋ค. ๋ฐ๋ผ์ ์ฝ๋๊ฐ `onclick` ํธ๋๋ฌ๋ฅผ ๋ฎ์ด ์ฐ๋ฉด ์๋ ํธ๋ค๋ฌ๋ ์์ํ ์์ค๋๋ค. ๊ทธ๋์ ๋ ๋์ ์ ๊ทผ ๋ฐฉ์์ธ `addEventListener` ๋ฉ์๋๋ฅผ ์ฌ์ฉํ๋ค.

`addEventListener`์ ์ฒซ ๋ฒ์งธ ๋งค๊ฐ๋ณ์๋ ์ด๋ฒคํธ ํ์์ด๊ณ , ๋๋ฒ์งธ ๋งค๊ฐ๋ณ์๋ ์ฝ๋ฐฑ์ด๋ฉฐ ์ด๋ฒคํธ๊ฐ ํธ๋ฆฌ๊ฑฐ๋  ๋ ํธ์ถ๋๋ค. ๋ค์ ์ฝ๋์ฒ๋ผ ๋ชจ๋  ํธ๋ค๋ฌ๋ฅผ ์ฐ๊ฒฐ ํ  ์ ์๋ค.

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

๋์๊ฐ `DOM`์ ์์๊ฐ ๋ ์ด์ ์กด์ฌํ์ง ์์ผ๋ฉด ๋ฉ๋ชจ๋ฆฌ ๋์๋ฅผ ๋ฐฉ์งํ๊ณ ์ ์ด๋ฒคํธ ๋ฆฌ์ค๋๋ฅผ ์ญ์ ํด์ผํ๋๋ฐ, ์ด๋ `removeEventListener` ๋ฉ์๋๋ฅผ ์ฌ์ฉํ๋ค. ๋ค์ ์ฝ๋์์ ๊ฐ์ฅ ์ค์ํ ์ ์ ์ด๋ฒคํธ ํธ๋ค๋ฌ๋ฅผ ์ ๊ฑฐํ๋ ค๋ฉด `removeEventListener`๋ฉ์๋์ ๋งค๊ฐ๋ณ์๋ก ์ ๋ฌํ  ์ ์๋๋ก ์ด์ ๋ํ ์ฐธ์กฐ๋ฅผ ์ ์งํด์ผ ํ๋ค๋ ๊ฒ์ด๋ค.

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
1. <a href='https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=260034588'>ํ๋ ์์ํฌ ์๋ ํ๋ก ํธ์๋ ๊ฐ๋ฐ - ํ๋์ธ์ค์ฝ ์คํธ๋ผ์ธจ๋ก</a>
2. https://developer.mozilla.org/ko/docs/Web/API/EventTarget/removeEventListener

---
### ๐ RegExp(์ ๊ทํํ์)์ ๊ฐ๋๊ณผ ์์ฉ ์์  ์ดํด๋ณด๊ธฐ
`RegExp`๋ ์ ๊ทํํ์์ด๋ผ๊ณ  ๋ถ๋ฅด๋๋ฐ ๋ฌธ์์ด์ ๋์์ผ๋ก ํจํด ๋งค์นญ ๊ธฐ๋ฅ์ ์ ๊ณตํ๋ค. ํจํด ๋งค์นญ ๊ธฐ๋ฅ์ด๋ ํน์  ํจํด๊ณผ ์ผ์นํ๋ ๋ฌธ์์ด์ ๊ฒ์ํ๊ฑฐ๋ ์ถ์ถ ๋๋ ์นํํ  ์ ์๋ ๊ธฐ๋ฅ์ ๋งํ๋ค. ์ฌ๋ฌ ๋ฐฉ๋ฉด์์๋ ์ฌ์ฉ๋์ง๋ง ๋๋ ์๊ณ ๋ฆฌ์ฆ ๋ฌธ์ ๋ฅผ ํ๊ฑฐ๋ `util` ๊ด๋ จ ํจ์๋ฅผ ๋ง๋ค ๋ ์ฃผ๋ก ์ฌ์ฉํ๋ค. ์ ๊ทํํ์์ ์ฅ์ ์ ๋ฐ๋ณต๋ฌธ๊ณผ ์กฐ๊ฑด๋ฌธ ์์ด ํจํด์ ์ ์ํ๊ณ  ํ์คํธํ๋ ๊ฒ์ผ๋ก ๊ฐ๋จํ ์ฒดํฌํ  ์ ์๋ค. ํ์ง๋ง, ์ฌ๋ฌ๊ฐ์ง ๊ธฐํธ๋ฅผ ํผํฉํ์ฌ ์ฌ์ฉํ๊ธฐ ๋๋ฌธ์ ์ฒ์ ์ ํ๋ค๋ฉด ๊ฐ๋์ฑ์ด ๋งค์ฐ ์ข์ง ๋ชปํ ๋จ์ ์ด ์๋ค. ์ ๊ทํํ์์๋ ๋ค์ํ ํจํด์ด ์์ง๋ง ๊ทธ ์ค ๋ํ์ ์ผ๋ก ์ฐ์ด๋ ๊ฒ๋ค๋ง ์์๋ณด์.

๋ณธ๋ก ์ผ๋ก ๋ค์ด๊ฐ๊ธฐ ์ ์ ์ ๊ท ํํ์์ ๊ฒ์ ๋ฐฉ์์ ์ค์ ํ๋ ํ๋๊ทธ๊ฐ ์๋๋ฐ, ์ด 6๊ฐ์ ํ๋๊ทธ๊ฐ ์๋ค. ํ๋๊ทธ๋ ์ฌ๋ฌ๊ฐ ๋ฃ์ ์ ์์ผ๋ฉฐ, ํ๋๊ทธ๋ฅผ ์ฌ์ฉํ์ง ์์ ๊ฒฝ์ฐ์๋ ๋์๋ฌธ์๋ฅผ ๊ตฌ๋ถํ์ฌ ํจํด์ ๊ฒ์ํ๋ค. ๋ํ ๊ฒ์ ๋งค์นญ ๋์์ด ์ฌ๋ฌ๊ฐ์ฌ๋ ์ฒซ ๋ฒ์งธ ๋งค์นญ ๋์๋ง ๊ฒ์ํ๊ณ  ์ข๋ฃํ๋ค.(์ฌ๋ฌ๊ฐ์ ๋งค์นญ ๊ฒฐ๊ณผ๋ฅผ ๋ฐํํ๋ ค๋ฉด `g`ํ๋๊ทธ๋ฅผ ์ฌ์ฉํ๋ฉด ๋๋ค.)

```javascript
1. `g`(global): ํจํด๊ณผ ์ผ์นํ๋ ๋ชจ๋  ๊ฐ๋ค์ ์ ์ญ ๊ฒ์ํ๋ค. (๋ฐ์ฑ๋ฌธ์ ์์ด๋ก ํด์ํ ๊ฒ์ด ์๋ ๐)
2. `i`(ignoreCase): ๋์๋ฌธ์๋ฅผ ๊ตฌ๋ถํ์ง ์๊ณ  ํจํด์ ๊ฒ์ํ๋ค.
3. `m`(multiLine): ๋ฌธ์์ด์ ํ์ด ๋ฐ๋๋๋ผ๋ ํจํด ๊ฒ์์ ์ง์ํ๋ค.
4. `s`(source): ๊ฐํ ๋ฌธ์์ธ `\n`๋ ํฌํจํ๋๋ก ํ์ฑํ ํ๋ค.
5. `u`(unicode): ์ ๋์ฝ๋ ์ ์ฒด๋ฅผ ์ง์ํ๋ค.
6. `y`(sticky): ๋ฌธ์ ๋ด ํน์  ์์น์์ ๊ฒ์์ ์งํํ๋ `sticky` ๋ชจ๋๋ฅผ ํ์ฑํ ์ํจ๋ค.
```

ํ๋๋ง ๋ ์์๋ณด์. ๋ฐ๋ก ํจํด์ธ๋ฐ, ํ๋๊ทธ๋ ์ ๊ท ํํ์์ ๊ฒ์ ๋ฐฉ์์ ์ค์ ํ๊ธฐ ์ํด ์ฌ์ฉ๋๋ค๋ฉด, ์ ๊ทํํ์์ ํจํด์ ๋ฌธ์์ด์ ์ผ์ ํ ๊ท์น์ ํํํ๊ธฐ ์ํด ์ฌ์ฉํ๋ค. ํจํด์ ๋ฌธ์์ด์ ๋ฐ์ดํ ๋์  `/`๋ก ์ด๊ณ  ๋ซ๋๋ค. ์ฆ, `"Hello"`๊ฐ ์๋๋ผ `/Hello/`๋ก ์ฒ๋ผ ์ฌ์ฉํ๋ ๊ฒ์ด๋ค. ๋ง์ฝ, ๋ฐ์ดํ๋ฅผ ํฌํจํ๋ค๋ฉด ๋ฐ์ดํ๊น์ง๋ ํจํด์ ํฌํจ๋๋ ์ด ์ ์ ์ฃผ์ํ์. ํจํด์ ์ข๋ฅ๋ ๋ง์ฐฌ๊ฐ์ง๋ก ์ฌ๋ฌ๊ฐ ์์ง๋ง ์ฃผ๋ก ์ฌ์ฉํ๋ ํจํด์ ๋ค์๊ณผ ๊ฐ๋ค.

1. `[ ]` : ๊ดํธ์์ ์๋ ๊ฐ์ `or` ์ ๋ํ๋
2. `\d`: [ 0-9 ]๋ฅผ ์๋ฏธํจ
3. `\D`: ์ซ์๊ฐ ์๋ ๋ฌธ์๋ฅผ ์๋ฏธํจ
4. `\w`: ์ํ๋ฒณ, ์ซ์, ์ธ๋์ค์ฝ์ด๋ฅผ ์๋ฏธํจ( `[ A-Za-z0-9 ]`)
5. `\W`: ์ํ๋ฒณ, ์ซ์, ์ธ๋์ค์ฝ์ด๊ฐ ์๋ ๋ฌธ์๋ฅผ ์๋ฏธํจ
6. `/s`: ์ฌ๋ฌ๊ฐ์ง ๊ณต๋ฐฑ ๋ฌธ์(์คํ์ด์ค, ํ ๋ฑ)๋ฅผ ์๋ฏธํ๋ค. `[/t/r/n/v/f]` ์ ๊ฐ์ ์๋ฏธ๋ค.
7. `*`: ์ด์  ํญ๋ชฉ์ 0๋ฒ ์ด์ ๋ฐ๋ณตํจ
8. `+`: ์ด์  ํญ๋ชฉ์ 1ํ ์ด์ ๋ฐ๋ณตํจ
9. `^`: `[ ]` ๋ด๋ถ์ ์๋ `^` ๋ `not` ์ ์๋ฏธํ๊ณ , `[ ]` ์ธ๋ถ์ ์๋ `^` ๋ ๋ฌธ์์ด์ ์์์ ์๋ฏธํ๋ค.
10. `$`: ๋ฌธ์์ด์ ๋ง์ง๋ง์ ์๋ฏธํ๋ค.
11. `?`: ์์  ํจํด์ด ์ต๋ ํ ๋ฒ ์ด์(0๋ฒ ํฌํจ) ๋ฐ๋ณต๋๋์ง๋ฅผ ์๋ฏธํ๋ค. ์ฆ, ์์  ํจํด (`s`)์ด ์๊ฑฐ๋ ์์ด๋ ๋งค์น๋๋ค.
12. `{m,n}`: ๋ฐ๋ณต ๊ฒ์ ํจํด์ผ๋ก ์ต์ `m`๋ฒ, ์ต๋ `n`๋ฒ ๋ฐ๋ณต๋๋ ๋ฌธ์์ด์ ์๋ฏธํ๋ค. ์ฝค๋ง๋ค์ ๊ณต๋ฐฑ์ด ์ค์ง ์๊ฒ ์ฃผ์ํ์.

์, ์ด์  ํ๋๊ทธ์ ํจํด์ ๋ํด์ ์์๋ดค์ผ๋ ๋ณธ๊ฒฉ์ ์ผ๋ก `RegExp` ๋ฉ์๋์ ๋ํด ์์๋ณด์. <a href='https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp'>MDN</a>์ ์ ํ์๋ฏ์ด ๋ฉ์๋๋ ์ฌ๋ฌ๊ฐ ์์ง๋ง, ์ฌ๊ธฐ์๋ `exec`, `match`, `test`์ ๋๋ง ์์๋ณด์. 

1. `exec`: ๋งค์นญ ๊ฒฐ๊ณผ๋ฅผ ๋ฐฐ์ด๋ก ๋ฐํํ๋ค. ๋งค์นญ ๊ฒฐ๊ณผ๊ฐ ์๋ ๊ฒฝ์ฐ `null`๋ก ๋ฐํํ๋ค. ํน์ด์ฌํญ์ผ๋ก๋ `g` ํ๋๊ทธ๋ฅผ ์ฌ์ฉํ๋๋ผ๋ ์ฒซ ๋ฒ์งธ ๋งค์นญ ๊ฒฐ๊ณผ๋ง ๋ฐํํ๋ค.

```javascript
let target = "is";
let str = "what is your name? my name is ywtechit";

console.log(/is/g.exec(target));
๐๐ฝ [ 'is', index: 0, input: 'is', groups: undefined ]
```

2. `match`: ์ฃผ์ด์ง ๋ฌธ์์ด์ ๋ํด ์ผ์นํ๋ ๊ฒฐ๊ณผ๋ฅผ ๋ฐํํ๋ค. `exec` ๋ฉ์๋์๋ ๋ค๋ฅด๊ฒ `g` ํ๋๊ทธ๋ฅผ ์ง์ ํ๋ฉด ๋ชจ๋  ๋งค์นญ ๊ฒฐ๊ณผ๋ฅผ ๋ฐฐ์ด๋ก ๋ฐํํ๋ค.

```javascript
// ex1: ๋ฌธ์ 'p'์ ๊ฐ์๋ฅผ return ํ์์ค
let lyrics = "i have a pen pineapple apple pen";
let reg = lyrics.match(/p/g) // [ 'p', 'p', 'p', 'p', 'p', 'p', 'p' ]
console.log(reg.length)
๐๐ฝ 7  // ๋ฐฐ์ด์ ๊ธธ์ด๊ฐ ๊ณง `p`์ ๊ฐ์์ ๋์ผํ๋ค.

let target = "what is your name? my name is ywtechit";

// ex2: is ๊ฐ์ ํ๋ฒ๋ง ์ฐพ๊ธฐ
console.log(target.match(/is/));
๐๐ฝ [ 'is', index: 5, input: 'what is your name? my name is ywtechit', groups: undefined ]

// ex3: is ๊ฐ์ ๋ชจ๋ ์ฐพ๊ธฐ
console.log(target.match(/is/g));
๐๐ฝ [ 'is', 'is' ]

// ex4: A๊ฐ 2๋ฒ์ด์ ๋ฐ๋ณตํ๋ ๋ฌธ์์ด์ ๋ฐํํ๊ธฐ
const target = "A AA B BB Aa Bb AAA";
const regExp = /A{2,}/g;
const result = target.match(regExp)
console.log('result :>> ', result);  // [ "AA", "AAA" ]

// ex5: URL ์ ์ผ ๋ง์ง๋ง ๊ฐ๋ง ์ถ์ถํ๋ ์ ๊ทํํ์
const url = "https://swtrack.elice.io/courses/16306/lecturerooms/16157";
const reg = url.match(/\/([0-9]+)$/);

console.log(reg);
๐๐ฝ
[
  0: '/16157',
  1: '16157',
  2: index: 51,
  3: input: 'https://swtrack.elice.io/courses/16306/lecturerooms/16157',
  4: groups: undefined
]

// ex6: URL์ ํฌํจ๋ ํน์  ๋ฌธ์ ๋ถ๋ฆฌํ๊ธฐ
let reg = s.match(/\/(lecturerooms)\/([0-9]+$)/);
console.log(reg);
๐๐ฝ
[
	0: "/lecturerooms/16157"
	1: "lecturerooms"
	2: "16157"
	groups: undefined
	index: 38
	input: "https://swtrack.elice.io/courses/16306/lecturerooms/16157"
	length: 3
]

// ex7: ๋ฌธ์์ ๋ด์ฉ๊ณผ ์๊ด์์ด 3์๋ฆฌ ๋ฌธ์์ด๊ณผ ๋งค์นํ๊ธฐ
let target = "Is this your mac? Is this your phone?";
console.log(target.match(/.../g));
๐๐ฝ
[
  'Is ', 'thi', 's y',
  'our', ' ma', 'c? ',
  'Is ', 'thi', 's y',
  'our', ' ph', 'one'
]
```

3. `test`: ๋งค์นญ๊ฒฐ๊ณผ๋ฅผ ๋ถ๋ฆฌ์ธ(boolean)๊ฐ์ผ๋ก ๋ฐํํ๋ค. ์ง๊ธ๊น์ง ์ด ๋ฉ์๋๋ฅผ ์ ์ผ ๋ง์ด ์ฌ์ฉํ๋ค. ์ฌ๋ด์ผ๋ก ์ฌ๋ฐ๋ฅธ ์ด๋ฉ์ผ ํ์์ธ์ง ๊ฒ์ฌํ๋ ๊ณต์ ํจํด(FC 5322 Official Standard)์ <a href='https://emailregex.com/'>emailregex.com</a>์์๋ ํ์ธ ํ  ์ ์๋ค.

```javascript
// ex1: ํด๋น ๋ฌธ์์ด์ด ์๋์ง ๊ฒ์ฌํ๊ธฐ
let target = "what is your name? my name is ywtechit";

console.log(/is/.test(target));  // true

console.log(/typescript/.test(target));  // false

// ex2: new ์ฐ์ฐ์๋ฅผ ์ฌ์ฉํ RegExp์ new ์ฐ์ฐ์๋ฅผ ์ฌ์ฉํ์ง ์์ RegExp
const str = 'table football';
const regex = new RegExp('foo*');
const sameRegex = /foo*/;
const globalRegex = new RegExp('foo*', 'g');

console.log(regex.test(str));  // true

console.log(globalRegex.lastIndex);  // 0

console.log(globalRegex.test(str));  // true

console.log(globalRegex.lastIndex);  // 9

console.log(globalRegex.test(str));  // false

// ex3: ์ฌ๋ฐ๋ฅธ ์ ํ๋ฒํธ ํ์์ธ์ง ํ๋ณํ๋ ํจํด
const tel = "010-1234-1234"
const regExp = /^\d{3}-\d{4}-\d{4}$/;
const result = regExp.test(tel);
console.log('result :>> ', result);  // true

// ex4: http://, https://๋ก ์์ํ๋ ๋๋ฉ์ธ์ธ์ง ํ๋ณํ๋ ํจํด
const domain = "https://www.naver.com";
const regExp = /^(http|https):\/\//;
const regExp2 = /^https?:\/\//;
const result = regExp.test(domain);
console.log('result :>> ', result);  // true

// ex5: html๋ก ๋๋๋ ํ์ผ๋ช์ธ์ง ํ๋ณํ๋ ํจํด
const string = "index.html";
const regExp = /html$/
const result = regExp.test(string)
console.log('result :>> ', result);  // true

// ex6: ์์์ ์ ์ต๋ 2๋ฒ๊น์ง๋ง ์ฌ์ฉํ๋ ์๋ ฅ์ธ๊ฐ?
const isUptoTwoDecimalPoint = (input: string): boolean => {
  const isIncludeDot = /[.]/g;

  if (isIncludeDot.test(input)) {
    const isTwoDecimalPoint = /^\d+[.]\d{1,2}$/;
    if (isTwoDecimalPoint.test(input)) return true;
    return false;
  }
  return true;
};

// ex7: ํ๋ ์ด์์ ๊ณต๋ฐฑ์ผ๋ก ์์ํ๋๊ฐ?
const string = " Hi!!";
const regExp = /^[\s]+/
const result = regExp.test(string)
console.log('result :>> ', result);  // true

// ex8: ์ํ๋ฒณ ๋์๋ฌธ์ ๋๋ ์ซ์๋ก ์์ํ๊ณ  ๋๋๋ฉฐ 4~10์๋ฆฌ์ธ๊ฐ?
const string = "abcd8637";
const regExp = /^[\w]{4,10}$/
const result = regExp.test(string)
console.log('result :>> ', result);  // true

// ex9: ์ฌ๋ฐ๋ฅธ ์ด๋ฉ์ผ ํ์์ธ๊ฐ?
const string = "ywtechit@gmail.com";
const regExp = /^[\w]([-_\.]?[\w])*@[\w]([-_\.]?[\w])*\.[a-zA-Z]{2,3}$/
const result = regExp.test(string)
console.log('result :>> ', result);  // trueโโโโโโat

// General Email Regex (RFC 5322 Official Standard)
const regExp = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;

// HTML5 input ํ๊ทธ์ type=โemailโ์ ์ฌ์ฉ๋๋ ํจํด(from W3C: Ref 5.)
<input type="email" placeholder="Enter your email" />
const regExp = /^[a-zA-Z0-9.!#$%&โ*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/

// ex10: ํน์๋ฌธ์ ํ๋ณํ๊ธฐ
let s = "(A(BC)D)EF(G(H)(IJ)K)LM(N)";
let answer = "";

console.log(solution(s));

function solution(s) {
  for (let x of s) {
    if (/[A-Z]/.test(x)) answer+=x;
  }
  return answer;
}

๐๐ฝ ABCDEFGHIJKLMN
```

4. `replace`: ์ฃผ์ด์ง ๋ฌธ์์ด๋ด์ ์ผ์น๋ฅผ ์๋ก์ด ๋ฌธ์์ด๋ก ๋์นํ๋ ๋ฉ์๋

```javascript
// ex1: ๊ณต๋ฐฑ ์ ๊ฑฐํ๊ธฐ
const str =
  "๋๋๋ง์ธ๋ฏธ ๋๊ท์ ๋ฌ์๋ฌธ์์๋ก ์๋ฅด ์ฌ๋ง๋ ์๋ํ ์ ์ด๋ฐ ์ ์ฐจ๋ก ์ด๋ฆฐ ๋ฐฑ์ฉ์ด ๋๋ฅด๊ณ ์ ธ ํ๋ฒ ์ด์๋ ๋ง์ฐธ๋ค ์  ๋จ๋ค ์๋ฌํด๋ ๋ชฏํง ๋ธ๋ฏธํ๋์ ๋ด ์ด๋ ์ํ์ผ ์ด์ฟ๋น๋๊ฒจ ์๋ก ์ค๋ฏ ์ฌ๋ซ ์ง๋ ๋งน๊ฐ๋ธ๋์ฌ๋๋ง๋ค ํด์ฌ ์๋น๋๊ฒจ ๋ ๋ก ์ค๋ฉ ๋ปํํ ํ๊ณ ์ ธ ํ ๋ฐ๋ผ๋ฏธ๋๋ผ";
const result = str.replace(/\s/g, "")

console.log('result :>> ', result);
๐๐ฝ ๋๋๋ง์ธ๋ฏธ๋๊ท์๋ฌ์๋ฌธ์์๋ก์๋ฅด์ฌ๋ง๋์๋ํ ์์ด๋ฐ์ ์ฐจ๋ก์ด๋ฆฐ๋ฐฑ์ฉ์ด๋๋ฅด๊ณ ์ ธํ๋ฒ ์ด์๋๋ง์ฐธ๋ค์ ๋จ๋ค์๋ฌํด๋๋ชฏํง๋ธ๋ฏธํ๋์๋ด์ด๋์ํ์ผ์ด์ฟ๋น๋๊ฒจ์๋ก์ค๋ฏ์ฌ๋ซ์ง๋๋งน๊ฐ๋ธ๋์ฌ๋๋ง๋คํด์ฌ์๋น๋๊ฒจ๋ ๋ก์ค๋ฉ๋ปํํํ๊ณ ์ ธํ ๋ฐ๋ผ๋ฏธ๋๋ผ

// ex2: ํน์๋ฌธ์ ์ ๊ฑฐํ๊ธฐ
let s = "found7, time: study; Yduts; emit, 7Dnuof";
let notIncludeSpecialCharacter = s.replace(/[^A-z]/g, '');
๐๐ฝ foundtimestudyYdutsemitDnuof

// ex3: ํน์๋ฌธ์ ์ค์์ ๊ณต๋ฐฑ์ ์ด๋ฆฌ๊ณ  ์ถ์ ๋
let notIncludeSpecialCharacter = s.replace(/[^A-z | " "]/g, '');
๐๐ฝ found time study yduts emit dnuof
```

์ง๊ธ๊น์ง ์ ๊ทํํ์์ ๊ฐ๋๊ณผ ์์ฉ ์์ ๋ฅผ ๊ฐ๋ตํ๊ฒ ์ดํด๋ณด์๋ค. ์ฒ์ ์ ๊ทํํ์์ ์ ํ์ ๋ ๋ํต ์ดํด๊ฐ ๋์ง ์์ ๋ฌธ๋ฒ์ด์๋๋ฐ, ๋ช๋ฒ ์ฌ์ฉํด๋ฒ๋ฆํ๋๊น ๋ฐ๋ณต๋ฌธ, ์กฐ๊ฑด๋ฌธ๋ณด๋ค ์ด์ฉํ๊ธฐ๊ฐ ์ฝ๊ณ  ํธํ๋ค. ์กฐ๊ธ์ฉ ์ ๊ทํํ์์ ์ฌ์ฉํ๋ฉฐ ๊ฒฝํ์ ์๊ณ  ๋์ค์ ํ์์ ๋์ํ๊ฒ๋๋ฉด ์์ ๋ฅผ ๊น๋จน์ง ์๋๋ก ๊ธฐ๋กํด๋ฌ์ผ ๊ฒ ๋ค.

Reference
1. https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp
2. https://ko.javascript.info/regexp-introduction
3. https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=251552545
4. https://emailregex.com/
5. https://html.spec.whatwg.org/multipage/input.html#input.email.attrs.value.multiple

---
### ๐ terminal์์ npm private ํจํค์ง๋ฅผ ๋ค์ด๋ฐ์ ์ ์์ ๋
`React`์์ `npm i`๋ฅผ ํตํด ํ์ํ ํจํค์ง๋ฅผ ๋ค์ด๋ฐ์ผ๋ ค๋๋ฐ `Not found` ์๋ฌ๊ฐ ๋จ๋ฉด์ ์ ์์ ์ผ๋ก ์ฐพ์ง ๋ชปํ๋ค. ๊ทธ๋์ <a href='https://www.npmjs.com/'>npm.js</a>์ ๋ค์ด๊ฐ๋ณด๋ ํ๋จ์ ์ฌ์ง์ฒ๋ผ ํจํค์ง๊ฐ ์ ์์ ์ผ๋ก ๋ฑ๋ก์ ๋์ด์์๋ค.

![](https://velog.velcdn.com/images/abcd8637/post/5008b76f-dc51-4a41-84b0-089373522388/image.png)

๊ฒฐ๊ตญ, `terminal`์์ `npm login`์ ํ์ง ์์์ ํจํค์ง๋ฅผ ๋ค์ด๋ฐ์ง ๋ชปํ ๊ฒ์ด์๋๋ฐ, ์๊ฐํด๋ณด๋ `private`ํ ํจํค์ง๋ฅผ ํ์ฌ ์์ฒญํ๋ ์ฌ์ฉ์์ ๊ถํ์ด ์๋์ง๋ ํ์ธํ์ง ์์ ์ฑ ๋ค์ด๋ก๋ ๋ฐ์ ์ ์๋ค๋ฉด ํจํค์ง๋ฅผ `private`๋ก ์ค์ ํ๊ฒ์ด ๋ฌด์จ ์์ฉ์ด ์์๊น๋ผ๋ ์๊ฐ์ด ๋ค์๋ค. `npm login` ์ปค๋งจ๋ ์๋ ฅ ํ ์์ ์ <a href='https://www.npmjs.com/'>npm.js</a>๊ณ์ ์ ์๋ ฅํ๋ฉด `Logged in as <id>`์ปค๋งจ๋๊ฐ ๋จ๋๋ฐ ์ ์์ ์ผ๋ก `terminal`์ `npm` ๊ณ์ ์ด ๋ฑ๋ก๋ ๊ฒ์ ํ์ธ ํ  ์ ์๋ค.

![](https://velog.velcdn.com/images/abcd8637/post/44b75373-9873-44c3-b478-92efe0f09883/image.png)

`terminal`์ ์์ ์ ๊ณ์ ์ด ์ฑ๊ณต์ ์ผ๋ก ๋ฑ๋ก๋๋์ง ํ์ธํ๋ ค๋ฉด `npm whoami`๋ช๋ น์ด๋ก ํ์ธํ์.

![](https://velog.velcdn.com/images/abcd8637/post/9b2ee914-0a26-4a97-b8d5-e01ea5f42cac/image.png)

Referenced
1. https://docs.w3cub.com/npm/getting-started/installing-node.html
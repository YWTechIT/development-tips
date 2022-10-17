## 📍 Object(...) is not a function 에러가 뜰 때
스토리북에 샘플 데이터를 추가하고 `npm run dev`명령어를 실행하니까 다음처럼 `Object(...) is not a function` 에러가 뜨는 현상을 마주했다.

![](https://res.cloudinary.com/ywtechit/image/upload/v1666009802/ywtechit/pkbboj1dupa71ftesrpk.png)

원인은 간단했는데, `@storybook/addon-knobs` 패키지에서 `import`한 `props` 문자열 타입을 `string`으로 사용했기 때문이다. 패키지를 뒤져보니 `typescript`에서 사용하는 `string` 타입은 `@storybook/addon-knobs` 패키지에서 `text` 타입으로 사용하기 때문이었다.(`export declare function text(name: string, value: string, groupId?: string): string;`) 문자형은 당연히 `string`인지 알았는데, `text` 타입이라니.. 당황스러웠다..

```typescript
// AS-IS
import React from 'react'
import { storiesOf } from '@storybook/react'
import { string } from '@storybook/addon-knobs'
import { DateInput } from '@titicaca/admin-input-components'

storiesOf('Date Input Component', module).add('Date Input', () => (
  <DateInput
    size={string('size', 'mini')}
    name={string('name', 'text')}
    value={string('value', '2022-10-24')}
  />
))

// TO-BE
import React from 'react'
import { storiesOf } from '@storybook/react'
import { text } from '@storybook/addon-knobs'
import { DateInput } from '@titicaca/admin-input-components'

storiesOf('Date Input Component', module).add('Date Input', () => (
  <DateInput
    size={text('size', 'mini')}
    name={text('name', 'text')}
    value={text('value', '2022-10-24')}
  />
))
```
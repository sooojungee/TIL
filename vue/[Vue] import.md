# [Vue] import

`import` 문은 외부 모듈이나 다른 스키릅트 등으로부터 export 된 기능을 가져오는데 사용된다.

## 문법

```js
import name from "modeule-name";
```

- `name`: 가져온 값을 받을 객체 이름
- `member, memberN`: export된 모듈에서 멤버의 이름만 가져옴
- `defaultMember`: export된 모듈의 default 이름을 지정
- `alias, aliasN`: 가져온 프로퍼티가 받을 객체의 이름을 지정
- `module-name`: 가져올 모듈이름/파일명


## 설명
`name` 파라미터는 export되는 멤버를 받을 오브젝트의 이름이다.
`member` 파라미터는 각각의 멤버를 지정한다.
`name` 파라미터는 모두를 가져온다.
모듈에서 name은 멤버 대신 하나의 default 파라미터를 통해 export하는 경우에도 동작할 수 있다.

```js
import * as myModule from "my-module.js";
```
모듈 전체를 가져온다.(*)
export한 모든 것들을 현재 범위(스크립트 파일 하나로 구분되는 모듈 범위) 내에 `myModule`로 바인되어 들어간다.

```js
import { myMember } from "my-module.js";
```
모듈에서 하나의 멤버만(myMember) 가져온다. 현재 범위 내에 `myMember`이 들어간다.

```js
import { foo, bar } from "my-module.js";
```
모듈에서 여러 멤버들(foo, bar)을 가져온다. 현재 범위내에 `foo`와 `bar`이 들어간다.

```js
import { reallyLongMouleMemberName as shortName } from "my-module.js";
```
멤버를 가져올 때 좀 더 편리한 별명을 지정해준다.
`reallyLongModuleMemberName`을 현재 범위내에서 `shortName`으로 사용할 수 있다.

```js
import {reallyLongModuleMemberName as shortName, anotherLongModuleName as short} from "my-module.js";
```
모듈에서 여러 멤버들을 편리한 별명을 지정하며 가져온다.

```js
import "my-module.js";
```
어떠한 바인딩 없이 모듈 전체의 사이드 이펙트만 가져온다.

### 기본값 가져오기

default export를 통해 내보낸 것으로 기본값을 가져올 수 있다.
export와 상반된 `import` 명령어를 통해 기본값을 가져오는 것이 가능하다.

```js
import myModule from "my-module.js";
```
기본값으로 바로 가져오는 가장 간단한 버전

```js
import myDefault, * as myModule from "my-module.js";
```
위와 함께 기본 구문도 같이 사용할 수 있다.
이러한 경우, 기본값을 가져오는 부분을 먼저 선언해야한다.
```js
import myDefault, { foo, bar } from "my-module.js";
```
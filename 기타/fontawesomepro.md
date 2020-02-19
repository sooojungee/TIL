## 설치

1. .npmrc 파일을 만든다.

```
@fortawesome:registry=https://npm.fontawesome.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

1. npm install
- 기본 설치

```
npm i --save @fortawesome/fontawesome-svg-core
npm i --save @fortawesome/vue-fontawesome
```

- pro 아이콘 설치 (free도 같이 쓸 수 있음)

```
npm i --save @fortawesome/pro-solid-svg-icons
npm i --save @fortawesome/pro-regular-svg-icons
npm i --save @fortawesome/pro-light-svg-icons
```

- brand 아이콘 설치

```
npm i --save @fortawesome/free-brands-svg-icons
```

## 사용하기

```
<font-awesome-icon :icon="['fas', 'user-secret' ]"/>
<font-awesome-icon :icon="['far', 'spinner' ]" />
```

```
import Vue from 'vue';
import { library } from "@fortawesome/fontawesome-svg-core";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

import { fas } from "@fortawesome/pro-solid-svg-icons";
import { far } from "@fortawesome/pro-regular-svg-icons";
import { fal } from "@fortawesome/pro-light-svg-icons";
import { fab } from "@fortawesome/free-brands-svg-icons";

library.add(fas, far, fal, fab);

Vue.config.productionTip = false;

Vue.component('font-awesome-icon', FontAwesomeIcon)
```

### 하나씩 사용

```
// app.vue의 js

import Vue from 'vue';
import { library } from '@fortawesome/fontawesome-svg-core'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

import { faUserSecret } from '@fortawesome/free-solid-svg-icons'
import { faSpinner } from '@fortawesome/pro-regular-svg-icons'

library.add(faUserSecret, faSpinner);
Vue.config.productionTip = false;

Vue.component('font-awesome-icon', FontAwesomeIcon)
```

### 한꺼번에 사용

```
import Vue from 'vue';
import HelloWorld from './components/HelloWorld'

import { library } from '@fortawesome/fontawesome-svg-core'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

import { fas } from '@fortawesome/free-solid-svg-icons'
import { far } from '@fortawesome/pro-regular-svg-icons'

library.add(fas, far);

Vue.config.productionTip = false;

Vue.component('font-awesome-icon', FontAwesomeIcon)
```

# Vuetify

```

```
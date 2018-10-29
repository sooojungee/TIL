# [Vue] 컴포넌트8 - 이름 규약, 재귀, 순환참조

### 컴포넌트 이름 규약

컴포넌트 (또는 prop)을 등록할 때, kebab-case, camelCase 또는 PascalCase를 사용할 수 있다.

> PascalCase
> 각 단어의 첫 문자를 대문자로 표기하되 붙여쓴다.
```js
components: {
	'kebab-cased-component': {},
	camelCasedComponent: {},
	PascalCasedComponent: {}
}
```

html 내에서는 kebab-case를 사용해야한다.

```pug
kebab-cased-component
camel-cased-component
pascal-cased-component
```

하지만 문자열 템플릿을 사용할 때 HTML의 대소문자를 구분하지 않는다.
즉, 템플릿에서도 CamelCase, PascalCase, kebab-case를 사용하여 컴포넌트와 prop을 참조할 수 있다.
- kebab-case
- camelCase를 사용하여 컴포넌트가 정의된 경우 camelCase 또는 kebab-case
- PascalCase를 사용하여 컴포넌트가 정의된 경우 kebab-case, camelCase 또는 PascalCase

```pug
// kebab-case를 사용하여 컴포넌트가 정의된 경우
kebab-cased-component

// camelCase를 사용하여 컴포넌트가 정의된 경우
camel-cased-component
camelCasedComponent

// PascalCase를 사용하여 컴포넌트가 정의된 경우
pascal-cased-component
pascalCasedComponent
PascalCasedComopoent
```
PascalCase는 가장 보편적인 선언적 컨벤션이고 kebab-case는 가장 보편적으로 사용하는 컨벤션이다.

컴포넌트가 `slot` 엘리먼트를 통해 내용을 전달받지 못하면 이름 뒤에 `/`를 사용하여 자체적으로 닫을 수도 있다.
```html
<my-component>
```
이것은 문자열 템플릿 내에서만 작동한다.


### 재귀 컴포넌트

컴포넌트는 `name`을 사용해 자신의 템플릿에서 재귀적으로 호출할 수 있다.
```js
name: 'unique-name-of-my-component'
```

`Vue.component`를 사용하여 컴포넌트를 전역적으로 등록하면, 글로벌 ID가 컴포넌트의 `name` 옵션으로 자동 설정된다.
```js
Vue.component('unique-name-of-my-component', {})
```

주의하지 않으면 재귀적 컴포넌트로 인해 무한 루프가 발생할 수도 있다.
```js
name: 'stack-overflow',
template: `<div><stack-overflow></stack-overflow></div>`
```
위와 같은 컴포넌트는 "최대 스택 크기 초과" 오류가 발생하므로 재기 호출이 조건부인지(마지막에 `false`가 될 `v-if`를 사용) 확인하여야 한다.

```pug
// item 재귀 컴포넌트
#item-template  
  li  
	 div(:class="{bold: isFolder}"  
	  @click="toggle"  
	  @dblclick="changeType") {{ model.name }}  
	  span(v-if="isFolder") [{{open ? '-' : '+'}}]  
  ul(v-show="open" v-if="isFolder")  
	  tree-component(v-for="model in model.children" :model = "model")  
	  li(class="add" @click="addChild")
```
```js
export default {  
  // name을 이용해 호출
  name: 'tree-component',
    
  props: {  
    model: Object  
  },  
  data() {  
    return {  
      open: false  
    };  
 },  
 computed: {  
  isFolder() {  
    return this.model.children && this.model.children.length;  
   }
 },
 methods: {  
  toggle() {  
    if (this.isFolder) this.open = !this.open;  
   },
   changeType() {  
      if (!this.isFolder) {  
        Vue.set(this.model, 'children', []);  
        this.addChild();  
        this.open = true;  
     }
  },
  addChild() {  
    this.model.children.push({  
      name: 'new stuff'  
    });  
   } 
 }
};
```

### 컴포넌트 사이의 순환 참조

파일 디렉토리 트리를 작성한다고 가정했을 때, 이 템플릿을 가지고 `tree-folder` 컴포넌트를 가질 수 있다.
```pug
// tree-folder.vue
p
	span {{ folder.name }}
	tree-folder-contents(:children="folder.children")
```

```pug
// tree-folder-contents.vue
ul
	li(v-for="child in children")
		tree-folder(v-if="child.children" : folder="child")
		sapn(v-else) {{ child.name }}
```
위의 두 개의 컴포넌트는 서로 자식 및 조상인 패러독스이다.
`Vue.component`를 이용해 전역적으로 컴포넌트를 등록할 때 이 패러독스는 자동으로 해결된다.

그러나 모듈 시스템을 사용하여 컴포넌트를 필요로하거나 가져오는 경우, Webpack 또는 Browserify를 통해 오류가 발생한다.
```
컴포넌트를 마운트하지 못했습니다 : 템플릿 또는 렌더링 함수가 정의되지 않았습니다.
```

두 개의 컴포넌트 A와 B가 있을 때,
모듈 시스템은 우선 A가 필요하다. 하지만 A는 B를 필요로 한다.
첫 번째 A의 의존성을 해결하지 않고서는 무한 루프에 빠져버린다.
이를 해결하려면 모듈 시스템에는 **A는 B를 필요로 하나 B를 먼저 해결할 필요가 없습니다.**라고 말 할 수 있는 지점을 제공해야 한다.

`tree-folder`을 그 지점을 삼으면, 패러독스를 만드는 자식은 `tree-folder-contents` 컴포넌트 이므로, `beforeCreate` 라이프 사이클 훅 시점까지 기다렸다가 해당 컴포넌트를 등록한다.
```js
beforeCreate() {
	this.$options.components.TreeFolderContents = require('./tree-folder-contents.vue');
}
```
또다른 방법으론, 컴포넌트를 지역등록하는 경우에 Webpack의 비동기 `import`를 이용할 수 있다.
```js
components: {
	TreeFolderContents: () => import('./tree-folder-contents.vue')
}
```

### 인라인 템플릿

하위 컴포넌트에 `inline-template`라는 특수한 속성이 존재하면, 컴포넌트는 그 내용을 분산된 내용으로 취급하지 않고 템플릿으로 사용한다.

```pug
my-component(inline-template='')
	div
		p 이것은 컴포넌트 자체 템플릿으로 컴파일 된다.
		p 부모가 만들어낸 내용이 아니다.
```
위와 같이 사용할 수 있다. 하지만 `inline-template`는 템플릿의 범위를 추론하기 어렵게 만든다. 
따라서  `template` 옵션을 사용하거나 `.vue` 파일의 `template` 엘리먼트를 사용하여 컴포넌트 내부에 템플릿을 정의하는 것이 좋다.

### X-Templates

템플릿을 정의하는 또 다른 방법은 `text-x-template` 유형의 스크립트 엘리먼트 내부에 ID로 템플릿을 참조하는 것이다.

```pug
script(type="text/x-template", id="hello-world-template")
	p child

h1 parent
	hello-world
```
```js
Vue.component('hello-world', {
	template: '#hello-world-template'
})
```
이 기능은 큰 템플릿이나 매우 작용 응용 프로그램의 데모에는 유용할 수 있지만 템플릿을 나머지 컴포넌트 정의와 분리하기 떄문에 피해야 한다.

### `v-once`를 이용한 적게드는 정적 컴포넌트

Vue에서 일반 HTML 엘리먼트를 렌더링 하는 것은 매우 빠르지만 가끔 정적 콘텐츠가 많이 포함된 컴포넌트가 있을 수 있다.
이런 경우, `v-once` 디렉티브를 루트 엘리먼트에 추가함으로써 캐시가 한 번만 실행되도록 할 수 있다.

> v-once
> 초기에 한 번만 렌더링 (데이터를 사용하지만 변동이 없는 정적인 부분을 보여줄때 사용)
```js
Vue.component('terms-of-service', {
	template: `\
	  <div v-once>\
	    <h1>Terms of Service</h1>\
	  </div>`
})
```
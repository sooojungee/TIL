# [Vue] 프로그래밍 방식으로 컴포넌트 인스턴트 만들기

```pug
<template>
	slot.button {{ text }}
</template>
<script>
export default {
	name: 'Button',
	props: {
		text: {
			type: String,
			default: 'text'
		}
	}
}
</script>
```

위와 같은 Vue.js 컴포넌트가 있을 때, 다른 요소에서 사용하기
다른 컴포넌트에 이 컴포넌트를 동적으로 사용하는 방법

## 인스턴스 만들기

요소 객체를 전달할 `Vue.extend`하여 Vue 생성자의 하위 클래스를 만드는 것이다.
```js
import Vue from 'vue';
import Button from 'Button';

const ComponentClass = Vue.extend(Button);
const instance = new ComponentClass();
```

## DOM에 삽입

```pug
#app
	.addButton(@click="addButton")
	.removeButton(@click="removeButton")
	.container(ref="container")
```
```js
import Vue from 'vue';
import Button from 'Button';

const ComponentClass = Vue.extend(Button);

data() {
	return {
		instance: null
	}
}
methods: {
	addButton() {
		this.instance = new ComponentClass();
		this.instance.$mount();
		this.$refs.container.appendChild(this.instance.$el);
	},
	removeButton() {
		this.$refs.container.removeChild(this.instance.$el);
		this.instance = null;
	}
}
```
Vue 구성요소 인스턴스에서 기본 DOM 요소 참조를 가져오려면 `$el`속성을 사용할 수 있다.


## prop 전달

`propsData`를 사용하여 prop을 전달한다.

```js
this.instance = new ComponentClass({
	propsData: { text: 'this text' }
});
this.instance.$mount();
this.$refs.container.appendChild(this.instance.$el);
```

## slot 설정하기

슬롯은 `$slots`속성의 모든 인스턴스에서 액세스 할 수 있다.
슬롯을 사용하지 않으려면 슬롯을 `$slots.default` 배열로 사용할 수 있다.

인스턴스를 마운트하기전에 수행해야 한다.

```js
this.instance = new ComponentClass({
	propsData: { text: 'this text' }
});
this.instance.$slots.default = ['click me'];
this.instance.$mount();
this.$refs.container.appendChild(this.instance.$el);
```


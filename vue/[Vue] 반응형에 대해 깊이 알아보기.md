# [Vue] 반응형에 대해 깊이 알아보기

## 변경 내용을 추적하는 방법

Vue 인스턴스에 JavaScript 객체를 `data` 옵션으로 전달하면 Vue는 모든 속성에 **Object.defineProperty**를 사용하여 getter/setter로 변환한다. 
이는 Vue가 ES5를 사용할 수 없는 IE8 이하를 지원하지 않는 이유이다.

getter/ setter는 사용자에게는 보이지 않으나 속성에 엑세스 하거나 수정할 때 Vue가 종속성 추적 및 변경 알림을 수행할 수 있다.
한 가지 주의 사항은 변환된 데이터 객체가 기록될 때 브라우저가 getter / setter 형식을 다르게 처리하므로 친숙한 인터페이스를 사용하기 위해 vue-devtools를 설치하는 것이 좋다.
> vue-devtools
> https://github.com/vuejs/vue-devtools 

모든 컴포넌트는 인스턴스에 해당 watcher가 있으며, 이 인스턴스는 컴포넌트가 종속적으로 렌더링 되는 동안 수정된 모든 속성을 기록한다. 나중에 종속적인 setter가 트리거 되면 watcher에 알리고 컴포넌트가 다시 렌더링된다.


### Object.defineProperty
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty

정적 메소드 Object.defineProperty()는 개체에 대한 새 속성을 직접 정의하거나 개체의 기존 속성을 수정하고 개체를 반환한다.
이 방법을 `Object` 유형의 인스턴스가 아니라 `Object` 생성자에서 직접 호출한다.

< 예시 >
```js
const object1 = {};

Object.defineProperty(object1, 'property1', {
	value: 42,
	writable: false
});

object1.property1 = 77;
// error

console.log(object1.property1);
```
 
 **1. 문법**
```
Object.defineProperty(obj, prop, descriptor)
```
1. `obj`: 프로퍼티를 정의할 객체
2. `prop`: 정의되거나 수정될 프로퍼티의 이름
3. `descriptor`: 정의되거나 수정될 프로퍼티의 서술구

**2. 설명**
이 메소드를 사용하면 개체의 속성을 정확하게 추가하거나 수정할 수 있다.
할당을 통해 일반 속성을 추가하면 프로퍼티 열거 중에 나타나는 속성이 되고 값이 변경되거나 삭데될 수 있다.
이 방법을 사용하려면 이러한 추가 세부 정보를 기본 값에서 변경할 수 있다.
기본적으로 `Object.defindeProperty()`를 사용하여 추가한 값은 변경할 수 없다.

Property descriptor은 두가지 주된  속성이 있다.
*data descriptor(데이터 설명자)*은 값이 있으며 쓰기 가능하거나 기록되지 않는 속성이다.
*accessor descriptor(접근자 설명자)*은 가져오기 기능 쌍에서 설명하는 속성이다.
descriptor은 이 두가지 속성 중 하나여야만 한다.

데이터와 접근 설명자 모두 개체이다. 이러한 키에는 아래와 같은 옵션 key가 있다.
- `configurable`
	이 속성의 설명자 유형을 변경할 수 있고 속성을 해당 객체에 삭제 할 수 있는 경우에 `true`
	default : `false`
	
- `enumerable`
	해당 개체의 속성 열거 중에 이 속성이 표시되는 경우에 `true`
	default: `false`

데이터 설명자가 가지고 있는 옵션 key
- `value`
	속성과 연결된 값이다. 유효한 JavaScript 값(숫자, 객체, 함수 등)이 될 수 있다.
	default: `undefined`
	
- `writable`
	할당 연산자를 사용하여 속성과 관련된 값을 변경할 수 있는 경우에 `true`
	default: `false`

접근 설명자가 가지고 있는 옵션 key
- `get`
	속성의 getter 역할을 하거나 getter가 없는 경우 정의되지 않은 함수이다.
	속성에 접근될 때, 함수는 인수없이 호출되고 이 함수를 속성에 접근할 개체로 설정한다.
	반환 값은 속성 값으로 사용된다.
	default: `undefined`
	
- `set`
	속성의 setter 역할을 하거나 setter가 없는 경우 정의 되지 않은 함수이다.
	속성이 할당된 경우, 이 기능은 하나의 인수와 이 값을 속성이 할당된 개체로 설정하여 호출된다.
	기본값이 정의되지 않음
	default: `undefined`

```js
function Archiver() {
	const temperature = null;
	const archive = [];
	Object.defineProperty(this, 'temperature', {
		get() {
			return temperature;
		},
		set(value) {
			temperature = value;
			archive.push({ value: temperature });
		}
	});
	this.getArchive = () => acthive
}
```

## 변경 감지 경고

최신 JavaScript의 한계로 인해 Vue는 속성의 추가 제거를 감지할 수 없다.
Vue는 인스턴스 초기화 중에 getter / setter 변환 프로세스를 수행하기 때문에 `data` 객체에 속성이 있어야 Vue가 이를 변환하고 응답할 수 있다.

```js
const vm = new Vue({
	data: {
		a: 1
	}
});
// vm.a 는 이제 반응적

vm.b = 2;
// vm.b는 이제 반응적이지 않음
```
Vue는 이미 만들어진 인스턴스에 새로운 루트 수준의 반응 속성을 동적으로 추가하는 것을 허용하지 않는다.
그러나 `Vue.set(object, key, value)` 메소드를 사용하여 중첩된 객체에 반응 속성을 추가할 수 있다.
```js
Vue.set(vm.someObject, 'b', 2);
```
때로는 `Object.assign()` 또는 `_.extend()`를 사용하여 기존 객체에 많은 속성을 할당할 수 있다.
그러나 객체에 추가된 새 속성은 변경 내용을 트리거하지 않는다.
이 경우 원본 객체와 mixin 객체의 속성을 사용하여 새 객체를 만든다.
```js
// `Object.assign(this.someObject, { a: 1, b: 2})` 대신에
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2});
```

## 반응형 속성 선언하기

Vue 루트 수준의 반응성 속성을 동적으로 추가할 수 없으므로 모든 루트 수준의 반응성 데이터 **속성을 빈 값으라도 초기에 선언하여 Vue 인스턴스를 초기화 해야한다.**

```js
const vm = new Vue({
	data: {
		message: ''
	},
	template: '<div>{{ message }}</div>'
});

vm.message = 'Hello';
```
이 제한 사항에는 기술적인 이유가 있다.
종속성 추적 시스템에서 엣지 케이스 클래스를 제거하고 Vue 인스턴스를 유형 검사 시스템으로 멋지게 만들 수 있다.
또한 모든 반응 속성을 미리 선언하면 나중에 다시 방문하거나 다른 개발자가 읽을 때 구성 요소 코드를 더 쉽게 이해할 수 있다.

## 비동기 갱신 큐

**Vue는 DOM 업데이트를 비동기로 한다.**
데이터 변경이 발견될 때마다 큐를 열고 같은 이벤트 루프에서 발생하는 모든 데이터 변경을 버퍼에 담는다.
같은 watcher가 여러 번 발생하면 대기열에서 한 번만 푸시된다.
이 버퍼링된 중복 제거는 불필요한 계산과 DOM 조작을 피하는 데 있어 중요하다.
그 다음 이벤트 루프 "tick"에서 Vue는 대기열을 비우고 실제(이미 중복 제거된) 작업을 수행한다.
내부적으로 Vue는 비동기 큐를 위해 네이티브 `Promise.then`와 `MessageChannel`을 시도하고 `setTimeout(fn, 0)`으로 돌아간다.

Vue.js가 데이터 변경 후 DOM 업데이트를 마칠 때까지 기다리려면 데이터가 변경된 직후에 `Vue.nextTick(callback)`을 사용할 수 있다. `callback`은 DOM이 업데이트 된 후에 호출된다.

```pug
#example {{ message }}
```
```js
const vm = new Vue({
	name: 'test',
	data: {
		message: '123'
	}
});

vm.message = 'new message'; // 데이터 번경
vm.$el.textContent === 'new message'; // false
Vue.nextTick(() => {
	vm.$el.textContent === 'new message' // true
})
```

`vm.$nextTick()`은 내부 컴포넌트들에 특히 유용하다.
전역 `Vue`가 필요 없고 콜백의 `this` 컨텍스트가 자동으로 현재 Vue 인스턴스에 바인드될 것이기 떄문이다.
```js
Vue.component('example', {
	template: '<span> {{ message }} </span>',
	data() {
		return {
			message: '갱신 안됨'
		}
	},
	methods: {
		updateMessage() {
			this.message = '갱신됨';
			console.log(this.$el.textContent);
			this.$nextTick(function() {
				console.log(this.$el.textContent);
			})
		}
	}	
})
```
### Vue.nextTick([callback, context])
- 전달인자:
	- `{Function} [callback]`
	- `{Object} [context]`
- 사용방법:
	다음 DOM 업데이트 사이클 이후 실행하는 콜백을 연기한다.
	DOM 업데이트를 기다리기 위해 일부 데이터를 변경한 직후 사용해야 한다. 
```js
// 데이터를 변경한다.
vm.msg = 'hello';
// 아직 DOM 업데이트가 되지 않음
Vue.nextTick(() => {
	// DOM이 업데이트 됨
});

// promise로 사용
Vue.nextTick().then(() => {
	// DOM이 업데이트 됨
})
```
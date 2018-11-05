# [Vue] 사용자 지정 디렉티브

Vue는 코어에 포함된 기본 디렉티브 세트(`v-model`과 `v-show`) 이외에도 사용자 정의 디렉티브를 등록할 수 있다.
Vue 2.0에서 코드 재사용 및 추상화의 기본 형식은 컴포넌트이다.
그러나 일반 엘리먼트에 하위 수준의 DOM에 액세스가 필요한 경우가 있을 수 있다.
이러한 경우 사용자 지정 디렉티브가 여전히 유용할 수 있다.

### input 엘리먼트와 focusing에 대한 예제 
```pug
input(v-focus)
```
```js
// 전역 사용자 정의 디렉티브 v-focus등록
Vue.directive('focus', {
	// 바인딩된 엘리먼트가 DOM에 삽입되었을 때
	inserted(el) {
		// 엘리먼트에 포커스를 준다.
		el.focus();
	}
});
```
지시어를 로컬로 등록하기 위해서 컴포넌트는 `directives` 옵션을 허용한다.
```js
directives: {
	focus: {
		inserted(el) {
			el.focus();
		}
	}
}
```
페이지가 로드되면 해당 엘리먼트는 포커스를 얻는다. (참고: `autofocus`는 모바일 사파리에서 작동하지 않는다.)
사실 이 페이지를 방문한 이후 다른 것을 클릭하지 않았다면 이 input 엘리먼트에 포커스가 되어있어야 한다.

## 훅 함수

디렉티브 정의 객체는 여러가지 훅 함수를 제공할 수 있다. (선택사항임)

- `bind`: 디렉티브가 처음 엘리먼트에 바인딩될 때 한번만 호출된다. 이곳에서 일회성 설정을 할 수 있다.
- `inserted`: 바인딩된 엘리먼트가 부모 노드에 삽입되었을 때 호출된다. (**요소가 상위 DOM에 삽입되면 훅이 발생한다.**)이것은 부모 노드 존재를 보장하며 반드시 document 내에 있는 것은 아니다.
- `update`: 포함하는 컴포넌트가 업데이트된 후 호출된다. 그러나 자식이 업데이트되기 전일 가능성이 있다. 디렉티브의 값은 변경되었거나 변경되지 않았을 수 있지만 바인딩의 현재 값과 이전값을 비교하여 불필요한 업데이트를 건너 뛸 수 있다. (**요소가 업데이트될 때 훅이 호출되지만 자식은 아직 업데이트 되지 않는다.**)
- `componentUpdated`: 포함하고 있는 컴포넌트와 그 자식들이 업데이트된 후에 호출된다. (**컴포넌트와 자식이 갱신되면 이 훅이 불린다.**)
- `unbind`: 디렉티브가 엘리먼트로부터 언바인딩된 경우에만 한 번 호출된다. (** 이 훅은 지시문이 제거되면 호출된다.**)

## 디렉티브 훅 전달인자

디렉티브 훅은 다음을 전달인자로 사용할 수 있다.

- `el`: 디렉티브가 바인딩된 엘리먼트, 이것을 사용하면 DOM을 조작할 수 있다.
- `binding`: 아래의 속성을 가진 객체
	- `name`: 디렉티브 이름, `v-`프리픽스가 없다.
	- `value`: 디렉티브에서 전달받은 값, 예를들어 `v-my-directive="1 + 1"`인 경우 `value`는 2이다. 
	- `oldValue`: 이전 값. `update`와 `componentUpdated`에서만 사용할 수 있다. 이를 통해 값이 변경되었는지 확인할 수 있다.
	- `expression`: 표현식 문자열. 예를들어 `v-my-directive="1 + 1"`인 경우 표현식은 `"1 + 1"`이다.
	- `arg`: 디렉티브의 전달인자가 있는 경우에만 존재한다. 예를들어 `v-my-directive="1+ 1"`이면 `"foo"`이다.
	- `modifiers`: 포함된 수식어 객체, 있는 경우에만 존재한다. 예를들어 `v-my-directive.foo.bar`이면 수식어 객체는 `{ foo: true, bar: true }`이다.
- `vnode`: Vue 컴파일러가 만든 버추얼 노드
- `oldVnode` : 이전 버추얼 노드. `update`와 `componentUpdated`에서만 사용할 수 있다.

> `el` 뿐만 아니라 모든 전달인자는 읽기전용으로 사용하여야 한다. 절대 변경하면 안된다. 훅을 통해 이 정보들을 전달하는 경우, 엘리먼트의 dataset을 이용하면 된다.

< 사용자 정의 디렉티브 예제 1 >

```pug
#hook-arguments-example(v-demo:foo.a.b="message")
```
```js
// 전역레벨에 directive를 등록할 떄는 directive 메서드를 사용한다.
Vue.directive('demo', {
	// 콜백 함수에 전달되는 파라미터는 
	// 1. 바인딩되는 엘리먼트인 el
	// 2. 바인딩된 값들인 binding
	// 3. 가상 노드를 나타내는 vnode
	bind(el, binding, vnode) {
		const s = JSON.stringify;
		el.innerHTML =
		'name: ' + s(binding.name) + '<br>' +
		'value: ' + s(binding.value) + '<br>' +
		'expression: ' + s(binding.expression) + '<br>' +
		'argument: ' + s(binding.arg) + '<br>' +
		'modifiers: ' + s(binding.modifiers) + '<br>' +
		'vnode keys: ' + Object.keys(vnode).join(', ') 
	}

});

export default {
	el: '#hook-artuments-example',
	data() {
		return {
			message: 'hello!'
		}
	}
}
```

## 함수 약어

많은 경우, bind와 update에 같은 동작이 필요할 수 있는데 (다른 훅은 신경쓸 필요가 없을 때), 이런 때에는 옵션 객체가 아니라 하나의 함수만 전달하면 된다. 

```js
Vue.directive('color-swatch', (el, binding) => {
	el.style.backgroundColor = binding.value;
});
```

## 객체 리터럴

디렉티브에 여러 값이 필요한 경우 JavaScript 객체 리터럴을 전달할 수도 있다.
```pug
div(v-demo="{ color: 'white', text: 'hello' }")
```
```js
Vue.directive('demo', (el, binding) => {
	console.log(binding.value.color);
	console.log(binding.value.text);
});
```

< 사용자 디렉티브 예제 2 >
```pug
p(v-tack="{ top: '40', left: '100' }") Test
```
```js
Vue.directive('tack', {
	bind(el, binding, vnode) {
		el.style.position = 'fixed';
		el.style.top = binding.value.top + 'px';
		el.style.left = binding.value.left + 'px';
	}
})
```

# [Vue] 컴포넌트6- 동적 컴포넌트

같은 마운트 포인트를 사용하고 예약된 `component`엘리먼트를 사용하여 여러 컴포넌트 간에 동적으로 트랜지션하고 `is` 속성에 동적으로 바인드 할 수 있다.

`component`를 사용해서 컴포넌트 파트를 동적으로 변화시킬 수 있다.
> 컴포넌트를 변경하면 매번 컴포넌트가 `Destroy`되고 새로 `create`된다.

```pug
component(:is="currentView")
```
```js
export default {
	name: 'test',
	data() {
		return {
			currentView: 'home'
		}
	},
	components: {
		home: {},
		posts: {},
		archive: {}
	}
}
```

컴포넌트 객체에 직접 바인딩이 가능하다.
```js
const Home = {
	template: `<p>Welcome home!</p>`
}

export default {
	name: 'test',
	data() {
		return {
			currentView: Home
		}
	}
}
```

### `keep-alive`

트랜지션된 컴포넌트를 메모리에 유지하여 상태를 보존하거나 다시 렌더링하지 않도록 하려면 동적 컴포넌트를 `keep-alive`엘리먼트에 래핑할 수 있다.

동적 컴포넌트를 감싸는 경우 `keep-alive`는 비활성 컴포넌트 인스턴스를 파괴하지 않고 캐시한다.
`transition`과 비슷하게 `keep-alive`는 추상 엘리먼트이다.
DOM 엘리먼트 자체는 렌더링 하지 않고 컴포넌트 부모 체인에는 나타나지 않는다.

컴포넌트가 `keep-alive` 내에서 토글될 때, `actived`와 `deactivated` 라이프 사이클 훅이 그에 따라 호출된다.

```pug
// 기본 사용
keep-alive
	component(:is="currentView")

// 여러개의 조건부 자식 컴포넌트
keep-alive
	comp-a(v-if="a > 1")
	comp-b(v-else)

// <transition>과 함께 사용
transition
	keep-alive
		component(:is="view")
```
1. 비활성화된 컴포넌트는 캐싱된다.
2. 주로 컴포넌트 상태를 보존하거나 재 렌더링을 피하는데 사용한다.
3. `keep-alive`는 한 개의 자식 컴포넌트가 토글되고 있는 경우에 사용한다.
4. 내부에 `v-for`가 있으면 작동하지 않는다.

**props**
- `include` : string 또는 RegExp 또는 Array, 일치하는 컴포넌트만 캐시함
- `exclude` : string 또는 RexExp 또는 Array, 일치하는 컴포넌트는 캐시하지 않음 

`include`와 `exclude` prop는 컴포넌트를 조건부 캐시할 수 있다.
```pug
// 콤마로 구분된 문자열
keep-alive(include="a, b")
	component(:is="View")
keep-alive(:include="/a|b/")
	component(:is="View")
keep-alive(:include="['a', 'b']")
	component(:is="View")
```

------------

### 재사용 가능한 컴포넌트 제작하기

컴포넌트를 작성할 때 나중에 다른 곳에서 다시 사용할 것인지에 대한 여부를 명심하는 것이 좋다.
일회용 컴포넌트: 단단히 결합되어도 괜찮음
재사용 컴포넌트: 깨끗한 공용 인터페이스를 정의해야함

Vue 컴포넌트의 API는 prop, 이벤트 및 슬롯의 세 부분으로 나뉜다.
- **Prop**는 외부 환경이 데이터를 컴포넌트로 전달하도록 허용한다.
- **이벤트**를 통해 컴포넌트가 외부 환경에서 사이드 이펙트를 발생할 수록 있도록 한다.
- **슬롯**을 사용하면 외부 환경에서 추가 컨텐츠가 포함된 컴포넌트를 작성할 수 있다.


### 자식 컴포넌트 참조

prop나 이벤트가 있엇음에도 불구하고 때때로 JavaScript로 하위 컴포넌트에 직접 액세스 해야 할 수도 있다.
이를 위해 `ref`를 이용하여 참조 컴포넌트의 ID를 자식 컴포넌트에 할당해야 한다.

```pug
#parent
	user-profile(ref="profile")
```
```js
child = this.$refs.profile;
```

`ref`가 `v-for`와 함께 사용될 때, 얻을 수 있는 `ref`는 데이터 소스를 미러링하는 자식 컴포넌트를 포함하는 배열이다.

**`$refs`는 컴포넌트가 렌더링 된 후에만 채워지므로 템플릿이나 계산된 속성에서 `$refs`를 사용하지 말아야 한다.**

--------------
### input에서 한글 입력할 때 입력값 바로 가져오기

```pug
input(v-model="inputContent" @keyup="inputTest")
```
```js
inputTest() {
	console.log(this.inputContent);
}
```
`v-model`을 사용해서 한글을 입력할 때,
<'안녕하세요' 입력 >
안
안녕
안녕하
안녕하세
('요'자를 입력하고 화살표키나 스페이스 등을 누르지 않으면 '안녕하세요'가 출력되지 않는다)

```pug
input(ref="inputContent" @keyup="inputTest")
```
```js
inputTest() {
	console.log(this.);
}
```
`ref`를 사용해서 한글을 입력할 때,
< '안녕' 입력 >
ㅇ
아
안
안ㄴ
안녀
안녕
(바로 입력된다.)
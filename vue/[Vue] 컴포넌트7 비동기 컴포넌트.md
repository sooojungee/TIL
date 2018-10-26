# [CSS] 컴포넌트7 비동기 컴포넌트

### 비동기 컴포넌트

응용 프로그램을 더 작은 덩어리로 나누고 필요할 때만 서버에 컴포넌트를 로드해야 하는 경우가 있다.
Vue를 사용하면 컴포넌트 정의를 비동기식으로 해결하는 팩토리 함수로 컴포넌트를 정의할 수 있다.
Vue는 컴포넌트가 실제로 렌더링되어야 할 때만 팩토리 기능을 트리거하고 이후의 렌더링을 위해 결과를 캐시한다.

```js
Vue.component('async-example', (resolve, reject) => {
	setTimeout(() => {
		resolve({
			template: '<div> I am async! </div>'
		})
	}, 1000);
});
```
팩토리 함수는 `resolve`를 콜백을 받는다. 이 콜백은 서버에서 컴포넌트 정의를 가져왔을 때 호출되어야 한다.
또한 `reject(reason)`을 호출하여 로드가 실패했음을 알릴 수 있다.

권장되는 방법 중 하나는 **Webpack의 코드 분할 기능**과 함께 비동기 컴포넌트를 사용하는 것이다.
```js
Vue.component('async-webpack-example', (resolve) => {
	// Webpack이 Ajax 요청을 통해 로드되는 번들로 작성된 코드를 자동으로 분리하도록 지시한다.
	require(['./my-async-component'], resolve);
});
```

팩토리 함수에서 **`Promise`**를 반환할 수도 있다.
```js
Vue.component('async-webpack-example', () => import('./my-async-component'));
```

지역등록을 사용하는 경우, `Promise`를 반환하는 함수를 제공할 수 있다.
```js
export default {
	components: {
		'my-component': () => import('/my-async-component')
	}
}
```

### 고급 비동기 컴포넌트

2.3.0 버전 이후로 비동기 컴포넌트 팩토리는 다음 형태의 객체를 반환할 수 있다.
```js
const AsyncComp = () => ({
	// 로드하는 동안 컴포넌트이다. 반드시 promise이어야 한다.
	component: import('./myComp.vue'),
	// 비동기 컴포넌트가 로드되는 동안 사용할 컴포넌트
	loading: LoadingComp,
	/// 실패했을 경우 사용하는 컴포넌트
	error: ErrorComp,
	// 로딩 컴포넌트를 보여주기 전 지연하는 정도. 기본값: 200ms
	delay: 200,
	// 시간이 초과되면 에러용 컴포넌트가 표시된다. 기본값: infinity
	timeout: 3000
})
```
`vue-router`에서 라우트 컴포넌트로 사용하는 경우, 라우트 네비게이션이 발생하기 전에 비동기 컴포넌트가 먼저 작동하기 때문에 이러한 특성은 무시된다. 라우트 컴포넌트에서 위 문법을 사용하려면 `vue-router` 2.4.0 이상을 사용해야한다.

-------------

### 코드 분할
https://webpack.js.org/guides/code-splitting/

코드를 필요에 따라 병렬로 로드할 수 있다.

코드 분할에는 일반적으로 세 가지 방법이 있다.
1. Entry Points (진입점) : `entry` 구성을 사용하여 코드를 수동으로 분할한다.
2. Prevent Duplication (중복방지) : `splitChunksPlugin`청크를 중복 제거한다.
3. Dynamic Imports (동적 가져오기): 모듈 내에서 인라인 함수 호출을 통해 코드를 분할한다.

- Dynamic Imports
`import`의 변형이라고 볼 수 있는 기능으로 런타임 환경에서 정적으로 모듈을 불러오는 것이 아니라 동적으로 모듈을 부를 수 있도록 해준다.
동적으로 `import`된 모듈은 Promise로 반환되며, `then()` API를 통해서 비동기로 로딩된 모듈에 접근할 수 있다.

- Prevent Duplication
Dynamic Import 기능을 사용하여 앱의 파일을 여러 개로 분리할 경우 각 파일에서 의존하고 있는 라이브러리 코드가 중복될 수 있다.
`splitChunksPlugin`으로 중복 제거한다.
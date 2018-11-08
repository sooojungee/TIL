# [VueRouter] 동적 라우트 매칭

주어진 패턴을 가진 라우트를 동일한 컴포넌트에 매핑해야하는 경우가 있다.
모든 사용자에 대해 동일한 레이아웃을 가지지만 다른 사용자 id로 렌더링 되어야 하는 `User` 컴포넌트가 있을 수 있다.
`vue-router`에서 우리는 경로에서 동적 세그먼트를 사용하여 다음을 실행할 수 있다.

```js
const router = new VueRouter({
	routes: [
		{ path: '/user/:id/', component: User }
	]
});
```
이 때 `/user/foo`와 `/user/bar` 같은 url은 모두 같은 경로에 매핑된다.
동적 세그먼트는 콜론 `:`으로 표시된다.
라우트가 일치하면 동적 세그먼트의 값은 모든 컴포넌트에서 `this.$route.params`로 표시된다.

`$route.params`외에도 `#route` 객체는 `$route.query`, `$route.hash` 등의 유용한 정보를 제공한다.


## Params 변경 사항에 반응하기

매개 변수와 함께 라우트를 사용할 때 주의해야할 점은 사용자가 `/user/foo`에서 `/user/bar`로 이동할 때 동일한 컴포넌트 인스턴스가 재사용된다.
두 라우트 모두 동일한 컴포넌트를 렌더링하므로 이전 인스턴스를 삭제한 다음 새 인스턴스를 만드는 것보다 효율적이다.
하지만 이는 또한 컴포넌트의 라이프 사이클 훅이 호출되지 않음을 의미한다.

동일한 컴포넌트의 params 변경 사항에 반응하려면 `$route` 객체를 보면 된다.

```js
export default {
	watch: {
		$route(to, from) {
			// 경로에 반응하여...
			// from 변경하기 전
			// to 변경한 후
		}
	}
}
```

`/user/foo` -> `/user/bar`로 이동할 때 `mounted`는 불리지 않는다.
`$route`로 변경사항을 작성하거나 `updated`를 사용해야한다.
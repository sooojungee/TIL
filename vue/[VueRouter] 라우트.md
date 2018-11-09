# [VueRouter] 라우트

## 중첩된 라우트

실제 앱 UI는 일반적으로 여러 단계로 중첩된 컴포넌트로 이루어져있다.
URL의 세그먼트가 중첩된 컴포넌트의 특정 구조와 일치한다는 것은 매우 일반적이다.

`vue-router`를 사용하면 중첩된 라우트 구성을 사용하여 이 관계를 표현하는 것이 매우 간단하다.

```pug
#app
	router-view
```
```js
const User = {
	template: '<div>User {{ $route.params.id }}</div>'
};
const router = new VueRouter({
	routes: [
		{ path: '/user/:id', component: User}
	]
});
```
여기에 있는 `<router-view>`는 최상이 outlet이다.
최상위 경로와 일치하는 컴포넌트를 렌더링한다.
비슷하게 렌더링된 컴포넌트는 자신의 중첩된 `<router-view>`를 포함할 수도 있다.
다음은 `User` 컴포넌트의 템플릿 안에 하나를 추가하는 예이다.
```js
const User = {
	template: `
		<div class="user">
			<h2>User {{ $route.params.id }} </h2>
			<router-view></router-view>
		</div>
	`	
};
```

컴포넌트를 렌더링 하려면 `children`을 사용해야 한다.
```js
const router = new VueRouter({
	routes: [
		{ path: '/user/:id', component: User,
		  children: [
			  {
				// /user/:id/profile과 일치 할 때
				// UserProfile은 User의 <router-view> 내에 렌더링된다.
			    path: 'profile',
			    component: UserProfile
			  }
		  ]
		}
	]
});
```

**`/`로 시작되는 중첩된 라우트는 루트 경로로 취급된다.**
이렇게하면 중첩된 URL을 사용하지 않고 컴포넌트 중첩을 활용할 수 있다.

`children` 옵션은 `routes`와 같은 라우트 설정 객체의 또 다른 배열이다.
따라서 필요한만큼의 중첩된 뷰를 유지할 수 있다.

위의 설정으로 , `/user/foo`를 방문했을 때 하위 라우트가 매치되지 않았기 때문에 `User`의 outlet에 아무것도 출력되지 않는다. 이 경우 빈 서브 루트 경로를 제공할 수 있다.

```js
const router = new VueRouter({
	routes: [
		{ path: '/user/:id', component: User,
		  children: [
			  { path: 'profile', component: UserProfile }
			  
				// /user/:id일 때 router-view에 UserHome이 렌더링 된다.
			  { path: ''. component: UserHome}
		  ]
		}
	]
});
```

## 프로그래밍 방식 네비게이션

라우터 인스턴스 메소드를 사용하여 네비게이션용 anchor태그와 같은 일을 수행할 수 있다. 

### `router.push(location, onComplet?, onAbort?)`
> Vue 인스턴스 내부에서 라우터 인스턴스에 `$router`로 액세스 할 수 있다. 그러므로 `this.$route.push`를 사용할 수 있다.

다른 url로 이동하려면 `router-push`를 사용한다.
이 메소드는 새로운 항목을 히스토리스택에 넣기 때문에 사용자가 뒤로가기 버튼을 클릭하면 이전 url로 이동하게된다.

이것은 `router-link`를 클릭할 때 내부적으로 호출되는 메소드이므로,
`<router-link: to="...">`를 클릭하면 `router.push(...)`를 호출하는 것과 같다.

- 선언적 방식: `router-link(:to="...")`
- 프로그래밍 방식: `router.push(...)`
	전달인자는 문자열 경로 또는 로케이션 디스크립터 객체가 될 수 있다.

```js
// 리터럴 string
router.push('home');

// object
router.push({ path: 'home' });

// 이름을 가지는 라우트
router.push({ name: 'user', params: { userId: 123 }});

// 쿼리와 함꼐 사용, 결과는 /register?plan=private 이다.
router.push({ path: 'register', query: { plan: 'private' }});
```

### `router.replace(location)`

`router.push`와 같은 역할을 하지만 유일한 차이는 새로운 히스토리 항목에 추가하지않고 탐색한다는 것이다. => 현재 항목을 대체함 (뒤로가기 버튼을 클릭하면 replace 되기 전 화면의 전 화면을 보여준다 => replace하면 다음페이지로 넘어가는게 아님)

- 선언적 방식: `router-link(:to="..." replace)`
- 프로그래밍 방식: `router.replace(...)`

### `router.go(n)`

이 메소드는 `window.history.go(n)`와 비슷하게 히스토리 스택에서 앞으로 또는 뒤로 이동하는 단계를 나타내는 하나의 정수를 매개 변수로 사용한다.

```js
// 한 단계 앞으로 간다. history.forward()와 같다.
router.go(1);

// 한 단계 뒤로 간다. history.back()와 같다.
router.go(-1);

// 3 단계 앞으로 갑니다.
router.go(3);

// 지정한 만큼의 기록이 없으면 자동으로 실패 합니다.
router.go(-100);
```

### History 조작

`router.push`, `router.replace`, `router.go`는 `window.history.pushState`, `window.history.replaceState`, `window.history.go`와 상응한다.


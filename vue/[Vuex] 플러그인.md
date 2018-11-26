# [Vuex] 플러그인

Vuex 저장소는 각 변이에 대한 훅을 노출하는 `plugins` 옵션을 허용한다.
Vuex 플러그인은 저장소를 유일한 전달인자로 받는 함수이다.

```js
const myPlugin = (store) => {
	// 저장소가 초기화될 때 불린다.
	store.subscribe((mutation, state) => {
		// 매 변이시마다 불린다.
		// 변이는 { type, payload } 포맷으로 제공된다.
	});
};
```
> **subscribe**
> - `subscribe(handler: Function): Function`
> 
> 저장소 변이를 구독한다. `handler`는 **모든 변이 이후 호출**되고 변이 디스크립터와 변이 상태를 전달인자로 받는다.
> ```js
> store.subscribe((mutations, state) => {
>	console.log(mutation.type);
>	console.log(mutation.payload);
> });
> ```
> 저장소 액션을 구독하는 방법은 **subscribeAction**을 사용한다. (https://vuex.vuejs.org/kr/api/#subscribeaction)

다음과 같이 사용할 수 있다.
```js
const store = new Vuex.Store({
	plugins: [myPlugin]
});
```

< 예시 >
```js
const myPlugin = (store) => {
	store.subscribe((mutations, state) => {
		if (mutations.type === 'showCount') state.count += 1;
	});
};

const store = new Vuex.Store({
	state: {
		count: 0
	},
	mutations: {
		showCount(state) {
			console.log(state.count);
		}
	},
	plugins: [myPlugin]
});
```
```js
this.$store.commit('showCount'); // 0
console.log(this.$store.state.count); // 1
```

## 상태 스냅샷 가져오기

때로는 플러그인이 상태의 *snapshot*을 얻고자 할 수 있으며, 또한 변이 이후 상태와 변이 이전 상태를 비교할 수 있다.
이를 위해서는 상태 객체에 대한 깊은 복사를 수행해야 한다.
```js
const myPluginWithSnapshot = (store) => {
	// let prevState = _.cloneDeep(store.state); 는 저장소가 초기화될 때 한번만 불린다.
	let prevState = _.cloneDeep(store.state);
	
	store.subscribe((mutation, state) => {
		let nextState = _.cloneDeep(state);
		// prevState와 nextState를 비교할 수 있다.
		// 다음 변이를 위한 상태 저장
		prevState = nextState;
	});
}
```

상태 스냅샷을 사용하는 플러그인은 개발 중에만 사용해야 한다.
webpack 또는 Browserify를 사용하는 경우 빌드 도구가 이를 처리할 수 있다.
```js
const sotre = new Vuex.Store({
	plugins: process.env.NODE_ENV !== 'production' ? [myPluginWithSnapshot] : [myPlugin]
});
```
배포를 위해서는 Browserify가 `process.env.NODE_ENV !== 'production'`의 값을 최종 빌드를 위해 `false`로 변환한다.

### development, product 환경 세팅하기

웹서비스를 개발할 때, 개발(development) 모드와 배포(production)모드에 따라 다른 결과가 나와야 하는 경우가 있다.
이러한 문제들을 해결하기 위해서, Node.js에서는 **NODE_ENV**라는 환경변수를 설정하여,
```js
if (process.env.NODE_ENV === 'production') {
	console.log('Productive Mode');
} else if (process.env.NODE_ENV === 'development') {
	console.log('Development Mode');
}
```
로 설정할 수 있다.

#### NODE_ENV 값을 설정하는 방법
1. Linux, Mac OS X 를 기준으로 terminal, shell 창에서
	`export NODE_ENV=production` 을 입력하면 설정할 수 있다.
	
2. window 운영체제에서 node.js를 구동할 경우
	`set NODE_ENV=production`을 입력하면 설정할 수 있다.
	
3. webpack으로 만든 페이지의 경우,
	3-1) 설정 파일(webpack.config.js)에서 변경하는 방법
	```js
	const webpack = require('webpack');
	module.exports = {
		plugins: [
			new webpack.optimize.UglifyJsPlugin({
				sourceMap: options.devtool &&  (options.devtool.indexOf("sourcemap")  >=  0  || options.devtool.indexOf("source-map")  >=  0)
			}),
			new webpack.DefinePlugin({
				'precess.env.NODE_ENV': JSON.stringify('production')
			})
		]
	};
	```
	webpack(2.2.1 기준)은 DefinePlugin과 UglifyJsPlugin 두 가지의 플러그인을 지원한다.
	DefinePlugin은 컴파일 할 코드에서 특정 문자열을 설정한 값으로 치환해주는 기능을 하고,
	UglifyJsPlugin은 컴파일한 결과 코드의 용량을 줄이고 읽기 어렵게 만드는 역할을 한다.
	
	3-2)  명령어에서 옵션을 주는 방법
	```
	webpack --optimize-minimize --define process.env.NODE_ENV="'production'" (equal with)
	webpack -p
	```

## 플러그인 내부에서 변이 커밋하기

플러그인은 상태를 직접 변이할 수 없다.
컴포넌트와 마찬가지로 변이를 커밋하여 변경을 트리거할 수 있다.

변이를 커밋함으로써 플러그인을 상요하여 데이터 소스를 저장소에 동기화할 수 있다.

< 예시 >
```js
const plugin = (store) => {
	store.subscribe((mutation, state) => {
		store.commit('test', state);
	}
};

const store = new Vuex.Store({
	state: {
		number: 0
	},
	mutations: {
		test(state) {
			state.number += 1;
			console.log(state.number);
		}
	}
});
```
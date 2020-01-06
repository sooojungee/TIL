- 컴포넌트를 완전히 재사용하고 캡슐화하는 방법

```jsx
function tick()  {
	const element = (
		<div>
			<h2> It is {new Date().toLocalTimeString()} </h2>
		</div>
	);
	ReactDOM.render(
		elemet,
		document.getELementById('root')
	)
}

setInterval(tick, 1000);
```

→ 시계 캡슐화

```jsx
function Clock(props) {
	return(
		<div>
			<h2> It is {props.date.ptoLocalTimeString()} </h2>
		</div>
	);
}

function tick() {
	ReactDOM.render(
		<Clock date={new Date()}/>,
		document.getElementById('root')
	);
}

setInterval(tick, 1000);
```

---

## 함수에서 클래스로 변환하기

1. `React.Component`를 확장하는 동일한 이름의 ES6 class를 생성한다.
2. `render()`라고 불리는 빈 메서드 추가
3. 함수의 내용을 `render()`메서드 안으로 옮김
4. `render()` 내용 안에 있는 `props`를 `this.props`로 변경한다.
5. 남아있는 빈 함수선언 삭제

```jsx
class Clock extends React.Component{
	render() {
		return(
			<div>
				<h2> It is {this.props.date.toLocalTimeString()} </h2>
			</div>
		);
	}
}

// ..아래는 같음
```

---

## 클래스에 로컬 State 추가하기

1. `render()` 메서드 안에 있는 `this.props.date`를 `this.state.date`로 변경
2. 초기 `this.state`를 지정하는 class constructor 추가
3. `<Clock />`요소에서 `date` prop을 삭제

```jsx
class Clock extends React.Component{
	constructor(props) {
		super(props);
		this.state = {
			date: new Date()
		};
	}

	render() {
		return(
			<div>
				<h2> It is {this.state.date.ptoLocalTimeString()} </h2>
			</div>
		);
	}
}

reactDOM.render(
	<Clock/>,
	document.getElementById('root')
);
```

---

## 생명주기 메서드를 클래스에 추가하기

Clock이 처음 DOM에 렌더링 될때마다 상태 변화시키기 ⇒ 마운팅

컴포넌트 클래스에서 특별한 메서드를 선언하여 컴포넌트가 마운트/ 언마운트될 때 일부 코드 동작시키기

```jsx
componentDidMount() {
	this.timerID = setInterval(
		() => this.tick(), 1000
	);
}

componentWillUnmount() {
	clearInterval(this.timerID)
}
```

`props`, `state`가 아닌 데이터에 흐름 안에 포함되지 않는 항목을 부가할 때, 수동적으로 부가적인 필드를 추가한다.

---

## State를 올바르게 사용하기

### 1. 직접 State를 수정하지 않음

```jsx
// Wrong
this.state.comment = 'Hello';

//Correct
this.setState({ comment: 'Hello' });
```

`setState`를 수정할 수 있는 공간은 `constructor`이다.

### 2. State 업데이트는 비동기적일 수 있다.

React는 성능을 위해 `setState`를 단일 업데이트로 한꺼번에 처리할 수 있다.

`this.props`와 `this.state`가 비동기적으로 업데이트 될 수 있기 때문에 다음 `state`를 계산할 때 해당 값에 의존해서는 안된다.

이를 해결하기 위해, 객체보다 함수를 인자로 사용하는 `setState()`를 사용한다.

이 함수는 이전 state를 첫 번째 인자로, 업데이트가 적용된 시점의 props를 두 번째 인자로 받아들인다.

```jsx
// Wrong
this.setState({
	counter: this.state.counter + this.props.increment
});

// Correct
this.setState((state, props) => ({
	counter: state.counter + props.increment
}));
```

### 3. State 업데이트는 병합된다.

`setState()`를 호출할 때 React는 제공한 객체를 현재 state로 병합한다.

```jsx
constructor(props) {
	super(props);
	this.state = {
		posts: [],
		comments: []
	};
}

componentDidMount() {
	fetchPosts().then(response => {
		this.setState({ posts: response.posts })
	})
	fetchComments().then(response => {
		this.setState({ comments: response.comments})
	})
}
```

---

## 데이터는 아래로 흐른다.

부모 / 자식 컴포넌트는 모두 특정 컴포넌트가 유/무 상태인지 알 수 없다.

state는 종종 캡슐화라고 불린다. state가 설정된 컴포넌트 이외에는 어떠한 컴포넌트에도 접근할 수 없다.

컴포넌트는 자신의 state를 자식 컴포넌트에 props로 전달 할 수 있다.

```jsx
<h2>
	It is {this.state.date.toLocalTimeString()}
</h2>

...

<FormattedDate date={this.state.date} />

//

function FormattedDate(props) {
	return <h2>It is {props.date.toLocalTimeString()}</h2>;
}
```

일반적으로 이를 'top-down' 또는 '단방향식' 데이터 흐름이라고 한다.

모든 state는 특정 컴포넌트가 소유하고 있고 그 아래에 있는 컴포넌트에만 영향을 미친다.


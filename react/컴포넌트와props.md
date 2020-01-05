> 컴포넌트를 사용하여 UI를 독립적이고 재사용한 부분으로 분리하고, 각 부분을 독립적으로 생각할 수 있다.

컴포넌트 → 개념상 자바스크립트의 함수와 비슷

props를 받고 ReactElement를 반환

---

## 함수형 및 클래스 컴포넌트

1. 함수형: 자바스크립트 함수로 컴포넌트 정의

```jsx
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>
}
```

위 함수는 props 객체 인수를 받고 React Element를 반환하기 때문에 React 컴포넌트이다.

2. ES6 Class 사용

```jsx
class Welcome extends React.Component {
	render() {
		return <h1>Hello, {this.props.name}</h1>
	}
}
```

---

## 컴포넌트 렌더링

요소에 유저가 정의한 컴포넌트를 나타낼 수 있다.

```jsx
const element = <Welcome name="Soo"/>
```

React가 유저가 정의한 컴포넌트를 나타내는 요소(element)를 볼 때, 이 객체를 'props'라고 한다.

```jsx
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>;
}

const element = <Welcom name="Sara" />;
ReactDOM.render(
	element,
	document.getElementById('root')
);
```

컴포넌트 이름은 항상 대문자로 시작한다.

---

## 컴포넌트 결합

컴포넌트는 출력될 때 다른 컴포넌트 참조가 가능하다.

```jsx
function Welcome(props) {
	return <h1> Hello, {props.name}</h1>;
}

function App(props) {
	return(
		<div>
			<Welcome name="Sara" />
			<Welcome name="Cahal" />
			<Welcome name="Edite" />
		</div>
	);
}

ReactDOM.render(
	<App/>,
	document.getElementById('root')
);
```

일반적으로, 새 React 앱은 단일 App컴포넌트를 최상위에 둔다.

---

## 컴포넌트 추출

```jsx
function Comment(props) {
	return(
		<div className="Comment">
			<div className="UserInfo">
				<img className="Avatar"
					src={props.author.avatarUrl}
					alt={props.author.name}
				/>
				<div className="UserInfo-name">
					{props.author.name}
				</div>
			</div>
			<div className="Comment-text"> {props.text}	</div>
			<div className="Comment-date"> {formatDate(props.date)} </div>
		</div>

	);
}
```

```jsx
function Avatar(props) {
	return(
		<img className="Avatar"
			src={props.user.avatarUrl}
			alt={props.user.name}
		/>
	);
}

function UserInfo(props) {
	<div className="UserInfo">
		<Avatar user={props.user}/>
		<div className="UserInfo-name">
			{props.user.name}
		</div>
	</div>
}

function Comment(props) {
	return(
		<div className="Comment">
			<UserInfo user={props.author} />
			<div className="Comment-text"> {props.text}	</div>
			<div className="Comment-date"> {formatDate(props.date)} </div>
		</div>

	);
}
```

### props는 읽기 전용

컴포넌트의 props는 수정할 수 없다.

모든 React 컴포넌트는 props와 관련한 동작을 할 때 순수 함수처럼 동작해야한다.
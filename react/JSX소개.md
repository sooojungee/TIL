> 자바스크립트 문법의 확장

```jsx
const element = <h1> Hello World </h1>
```

이 문법을 **JSX** 라고 부른다.

이 element는 DOM에 rendering 된다.

React는 렌더링 로직이 다른 UI로직과 본질적으로 결합되어있다.

React는 별도의 파일에 마크업 로직을 넣어 기술을 인위적으로 분리하는 대신 '컴포넌트'로 관심사를 분리한다.

---

## JSX 표현식

- JSX에 표현식 포함

JSX 안에 자바스크립트 표현식을 중괄호로 묶어서 포함시킨다.

```jsx
const u = 'my name';
const element = (
	<div> {u} </div>
)
ReactDOM.render(
	element,
	document.getElementById('root')
);
```

- JSX 또한 표현식이다.

컴파일이 끝나면 JSX 표현식이 자바스크립트 함수 호출이 되고, 자바스크립트 객체로 인식된다.

따라서, if나 for내에서 JSX를 사용할 수 있고, 변수에 할당하거나 매개변수로 전달/ 반환 할 수 있다.

```jsx
function getGreeting(user) {
	if (user) {
		return <h1>Hello, {user}</h1>
	} else {
		return <h1>Hello, Stranger</h1>
	}
}
```

---

## JSX 속성

- 따옴표를 이용해 문자 리터럴 정의
- 중괄호를 이용해 자바스크립트 표현식 포함

```jsx
const element1 = <div tabIndex="0"></div>
const element2 = <div src={user.url}></div>
```

### - 자식 정의

JSX 태그는 자식을 가질 수 있다.

### - JSX 인젝션 공격 예방

유저 입력을 JSX 내에 포함시키는 것이 안전하다.

```jsx
const title = response.potentiallyMaliciousInput;
const element = <h1> {title} </h1>
```

React DOM은 렌더링되기 전에 JSX 내에 포함된 모든 값을 이스케이프 한다.

따라서 App에 명시적으로 작성되지 않은 내용은 삽입할 수 없다.

모든 것은 렌더링 되기 전에 문자열로 변환된다.

→ 이렇게 하면 XSS(클라이언트 측 스크립트를 다른 사용자가 보는 웹페이지에 삽입) 를 막을 수 있다.

### - JSX 객체 표현

Babel은 JSX를 React.createElement() 호출로 컴파일한다.

```jsx
const element = (
	<h1 className="greeting">
		hello, world!
	</h1>
);

// 위와 아래는 같은 코드

const element = React.createElement(
	'h1',
	{className: 'greeting'},
	'hello world'
);

// => 아래와 같은 객체를 생성한다. "React elements"

const element = {
	type: 'h1',
	props: {
		className: 'greeting',
		children: 'hello, world'
	}
};
```
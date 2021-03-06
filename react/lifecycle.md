--

## 생명주기 (lifecycle)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1038ed36-ee23-46d5-b58a-a33e3030e547/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1038ed36-ee23-46d5-b58a-a33e3030e547/Untitled.png)

1. `**componentWillMount()**`

- React element를 실제 DOM에 추가하기 직전에 호출되는 메소드

DOM이 생성되지 않았으므로 DOM을 조작할 수 없다.

`render`가 호출되기 전이기 때문에 `setState`를 사용해도 `render` 호출되지 않는다.

2. `**componentDidMount()**`

- 컴포넌트가 만들어지고 `render`가 호출된 이후에 호출되는 메소드

컴포넌트 출력물이 DOM에 렌더링 된 후 실행 = vue의 `mounted()`

Ajax나 타이머를 생성하고 코드를 작성하는 부분

3. `**componentWillReciveProps(nextProps)**`

- 컴포넌트 생성 후에 첫 렌더링을 마친 후 호출 되는 메소드

컴포넌트가 처음 마운트 되는 시점에서는 호출 X

`props`를 받아서 `state`를 변경해야하는 경우에 유용하다

→ `*getDerivedStateFromProps(nextProps, prevState)*`

- 컴포넌트가 인스턴스화된 후, 새 `props`를 받았을 때 호출되는 메소드

`setState`를 사용하는 것이 아닌 값을 `return` 으로 해야한다.

`state`를 갱신하는 객체를 반환할 수 있고, 갱신이 필요없을 때 `null`을 반환할 수 있다.

```jsx
componentWillReceiveProps(nextProps) {
	if (this.props.name !== nextProps.name) {
		this.setState({name: nextProps.name});
	}
}

static getDerivedStateFromProps(nextProps, prevState) {
	return prevState.name !== nextProps.name ? { name: nextProps.name } : null;
}
```

4. `**shouldComponentUpdate(nextProps, nextState)**`

- 컴포넌트 업데이트 직전에 호출되는 메소드

`props` 또는 `state`가 변경되었을 때, 재 렌더링 여부 계산한다.

5. `**componentWillUpdate(nextProps, nextState)**`

- `shouldComponentUpdate`가 호출된 후 컴포넌트 업데이트 직전에 호출되는 메소드

새로운 `props` 또는 `State`가 반영되기 직전 새로운 값을 받는다.

이 메서드안에서 `this.state()`를 사용하면 무한 루프 발생

→ `*getSnapshotBeforeupdate()*`

- DOM이 업데이트 직전에 호출된다.

```jsx
componentWillUpdate(nextProps, nextState) {
	if (this.props.list.length < nextProps.list.length) {
		this.previousScrollOffset = this.listRef.scrollHeight - this.listref.scrollTop;
	}
}

getSnapshotBeforeUpdate(prevProps, prevState) {
	if (prevProps.list.length < this.props.list.length) {
	  return (
	    this.listRef.scrollHeight - this.listRef.scrollTop
	  );
	}
	return null;
}
```

6. `**componentDidUpdate()**`

- 컴포넌트 업데이트 직후에 호출되는 메소드

7. `**componentWillUnmount()**`

- 컴포넌트가 소멸 (DOM에서 삭제된 후) 실행되는 메소드

컴포넌트 내부에서 타이머, 비동기 API 사용할 때, 이를 제거하기에 유용함

+ 8. `**componentDidCatch()**`

- 컴포넌트 오류 처리 개선을 위해 추가된 메소드

에러 발생 시에 `state`를 변경하고 `render`에서 해당 처리를 구현하면 된다.

```jsx
class ErrorBoundary extends React.Component {
	constructor(props) {
		super(props);
		this.state = { hassError: false };
	}

	componentDidCatch(error, info) {
		this.setState({ hasError: true });
		logErrorToMyService(error, info);
	}

	render() {
		if (this.state.hasError) {
			return <h1>Something went wrong./<h1>
		}
		return this.props.children;
	}
}
```
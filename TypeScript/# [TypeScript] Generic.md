# [TypeScript] Generic

### 정의
제네릭은 모든 타입의 객체를 다루면서도 객체 타입이 무결성을 유지하는 코드를 작성하는 방법이다.

## Hello World of Generics

파라미터로 입력된 모든 것을 되돌리는 함수 `identify`를 만든다고 할 때,
Generic이 없다면 `identify`에 함수에 특정 타입을 부여해야한다.

```ts
function identity(arg: any): any {
	return arg;
}
```
`any`타입을 사용하면 모든 타입을 수용할 수 있지만 실제로 함수가 리턴할 때 그 타입이 무엇인지 정보를 잃는다.

리턴되는 값을 나타내는데 사용하는 방식으로 파라미터 타입은 Capture하는 방법이 필요하다.
여기서 **Type변수**를 사용한다.
```ts
function identity<T>(arg: T): T {
	return arg;
}
```
이제 `identity` 함수에 Type변수 `T`를 추가했다.
`T`는 사용자가 제공한 타입을 캡쳐하여 나중에 해당 정보를 사용할 수 있도록한다.
그리고 리턴타입으로 `T`를 다시 사용한다.

이 버전의 `identity` 함수는 Generic타입이라고 말한다.
`any`를 사용하는 것과 달리, 파라미터와 리턴 값에 숫자를 사용하는 첫 번째 `identity`함수로서 어떤 정보도 잃지 않는다.

- 함수호출하는 방법
<1. Type 파라미터를 포함한 모든 파라미터를 함수에 전달하기>
```ts
let output = identity<string>("myString");
// 리턴 타입은 string
```
함수 호출에 대한 파라미터 중 하나인 `T`를 `string`으로 명시적으로 설정했다.

<2. 파라미터 타입에 reference를 사용하기 >
```ts
let output = identity("myString");
// 리턴 타입은 string 타입일 것이다.
```
컴파일러가 전달하는 타입에 따라 자동으로 `T`의 값을 설정한다.
**꺽쇠 괄호 (`<>`)에 명시적으로 타입을 전달할 필요가 없다.**

## `Generic Type` 변수로 작업하기

Generic을 사용하여 `identity`와 같은 Generic함수를 만들면 컴파일러는 함수의 몸체에 일반적으로 타입이 지정된 파라미터를 올바르게 적용하도록 강제한다.
즉, 이러한 파라미터 변수를 모든 타입이 될 수 잇는 것처럼 취급한다.

- 호출과 함께 파라미터에 `arg`의 길이를 기록하고 싶을 때
```ts
function loggingIdentity<T>(arg: T): T {
	console.log(arg.length);	// Error: T는 .length를 가지고 있지 않습니다.
	return arg;
}
```
컴파일러는 `arg`에 `.length`가 있다고 말할 수 없다라는 오류를 출력한다.

실제로 이 함수가 `T` 대신 `T` 배열로 직접 작업한다고 가정했을 때,
우리는 배열을 다루기 때문에 `.length` 멤버를 사용할 수 있어야 한다.
```ts
function loggingIdentity<T>(arg: T[]): T[] {
	console.log(arg.length);
	return arg;
}
```
1. Generic 함수 `loggingIdentity`는 타입파라미터 변수 `T`를 취한다.
2. 파라미터 `arg`는 `T`의 배열이며, `T`의 배열을 반환한다.
숫자 배열을 건네주면 `T`가 `number`에 묶이기 때문에 숫자 배열을 리턴할 것이다.
이렇게 하면 전체 타입보다는 일반타입 변수 `T`를 사용중인 타입의 일부로 사용할 수 있으므로 유연성이 향상된다.

혹은 다음과 같이 작성할 수 있다.
```ts
function loggingIdentity<T>(arg: Array<T>): Array<T> {
	console.log(arg.length);
	return arg;
}
```

## Generic 타입

Generic함수 타입과 Generic 인터페이스 만드는 방법

Generic함수 타입은 함수 선언과 마찬가지로 타입 파라미터가 먼저 나열된 Non-generic 함수의 형식과 같다.
```ts
function identity<T>(arg: T): T {
	return arg;
}

let myIdentity: <T>(arg: T) => T = identity;
// let myIdentity = identity;
```

타입의 수와 타입의 변수 사용법이 일치하면, 타입에서 Generic Type 파라미터에 다른 이름을 사용할 수 있다.
```ts
function identity<T>(arg: T): T {
	return arg;
}

let myIdentity: <U>(arg: U) => U = identity;
```

Generic타입을 객체 리터럴 형식의 Call signature로 쓸 수 있다.
```ts
function identity<T>(arg: T): T {
	return arg;
}

let myIdentity: {<T>(arg: T): T} = identity;
```

### Generic 인터페이스

```ts
interface GenericIdentityFn {
	<T>(arg: T): T;
}

function identity<T>(arg: T): T {
	return arg;
}

let myIdentity: GenericIdentityFn = identity;
```

**Generic 파라미터를 전체 인터페이스의 파라미터로 이동할 수 있다.**
이렇게 하면 Generic이 어떤 타입인지 알 수 있다.
```ts
interface GeneficIdentityFn<T> {
	(arg: T): T;
}

function identity<T>(arg: T): T {
	return arg;
}

let myidentity: GenericIdentityFn<number> = identity;
```
`GenericIdentityFn`을 사용할 때 해당 타입 파라미터를 지정해야하며 기본 Call signature가 사용할 항목을 효과적으로 잠글 수 있다.
타입 파라미터를 Call signature에 직접 적용할 시기와 인터페이스 자체에 넣을 시기를 이해하면 타입의 어떤 측면이 Generic인지 설명하는데 도움이된다.

**Generic Enum 및 네임스페이스는 만들 수 없다.**

## Generic 클래스

Generic 클래스는 클래스 이름 다음에 꺾쇠 괄호(<>)로 묶인 Generic Type 파라미터 목록을 갖는다.
```ts
class GenericNumber<T> {
	zeroValue: T;
	add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```
1. `GenericNumber` 클래스를 그대로 사용하지만, `number` 타입만 사용하도록하는 제한이 없다.

```ts
let stringNumeric = new GenericNumber<string>();
stringNumeric.zeroValue = "";
stringNumeric.add = function(x, y) { return x + y; }

alert(stringNumeric.add(stringNumeric.zeroValue, "test"));
```
인터페이스와 마찬가지로 Type 파라미터를 클래스 자체에 두면 클래스의 모든 프로퍼티가 동일한 타입으로 작동하는지 확인할 수 있다.

> 클래스에는 Static 측면과 Instance 측면의 두 가지 유형이 있다.
> Generic 클래스는 Static 측면보다 Instance 측면에서 Generic하기 때문에 **클래스로 작업할 때 Static 멤버는 클래스의 Type파라미터를 사용할 수 없다.**



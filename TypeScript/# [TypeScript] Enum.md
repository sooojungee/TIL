# [TypeScript] Enum

## Enums

### Numeric enums

enum을 사용하면 이름이 부여된 상수 집합을 정의할 수 있다.
```ts
enum Direction {
	Up = 1,
	Down,
	Left,
	Right
}
```
- enum의 본문은 0개 이상의 enum 멤버로 구성된다.
- enum 멤버는 관련된 숫자 값을 가지며 상수 또는 제한된 값이다.
- `Up`은 1, `Down` = 2, `Left` = 3, `Right` = 4

```ts
enum Response {
	No = 0,
	Yes = 1
}

function respond(recipient: string, message: Response): void {

}

respond("Princess Caroline", Response.Yes);
```

### String enums

문자열 열거형은 비슷한 개념이지만 약간의 런타임 차이가 있다.
문자열 열거형에서 각 멤버는 문자열 리터럴 또는 다른 문자열 열거형 멤버로 상수 초기화 되어야한다.
```ts
enum Direction {
	Up = "UP",
	Down = "DOWN",
	Left = "LEFT",
	Right = "RIGHT"
}
```
문자열 열거형은 자동 증가를 하지 않지만 "직렬화"를 잘한다.
즉, 디버깅 중이며 숫자 열거형의 런타임 값을 읽어야하는 경우 값이 종종 불투명하다.
문자열 열거형을 사용하면 열거형 멤버 자체의 이름과는 독립적으로 코드가실행될 때 의미 있고 읽기 쉬운 값을 제공해야한다.

## Heterogeneous enums

enum은 문자열과 숫자가 섞일 수 있지만 굳이 그렇게 사용할 필요는 없다.
```ts
enum BooleanLikeHeterogeneousEnum {
	No = 0,
	Yes = "YES"
}
```

## Computed and constant members

각 열거형 멤버에는 상수 또는 계산식이 될 수 있는 값이 있다.

- 열거 형의 첫 번째 멤버이며 초기화되지 않으면 0 이 할당된다.
	```ts
	enum E { X }
	// X = 0
	```
- 초기화 코드가 없고 앞의 멤버가 상수일 때 이전 멤버 값에 1을 더한 값이 현재 값이된다.
	```ts
	enum E1 { X, Y, Z }
	enum E2 {
		A = 1, B, C
	}
	// E1.X = 0, E1.Y = 1, E1.Z = 2
	// E2.A = 1, E2.B = 2, E2.C = 3
	```
- enum 멤버는 상수 enum expression으로 초기화된다. 상수 enum은 컴파일 타임에 완전히 평가할 수 있는 TypeScript expression의 하위집합니다. Expression이 다음중 하나일 경우 상수 enum이다.
	1. 숫자 리터럴
	2. 이전에 정의된 상수 enum멤버에 대한 참조
	3. 괄호로 묶인 상수 열거 expression
	4. 상수 enum expression에 적용된 단항 연산자 +, -, ~
	5. 이진연산자 +, -, *, /, %, <<, >>, >>>, &, |, ^ 이 상수 enum expression의 피연산저로 사용되는 경우 `Nan` 또는 `Infinity`로 평가되면 컴파일 타임 오류이다.

```ts
enum fileAccess {
	None,	// 0
	Read = 1 << 1,	// 2
	Write = 1 << 2,	// 4
	ReadWrite = Read | Write,	// 6
	G = "123".length	// 3
}
```

## Union enums and enum member types

계산되지 않은 상수 열거형 멤버의 특수 하위집합이있다.
리터럴 열거형 멤버는 초기화된 값이 없거나 다음 값으로 초기화된 값이 있는 상수열거형 멤버이다.
- 리터럴 문자열 ("foo", "bar", "baz")
- 모든 숫자 문자(1, 10)
- 모든 숫자 문자에 적용되는 단항 마이너스 (-1, -10)
열거형의 모든 멤버가 리터럴 열거형 값을 가지면 일부 특수한 의미가 재생된다.

열거형 멤버도 유형이된다.
```ts
enum ShapeKind {
	Circle,
	Square
}
interface Circle {
	kind: ShapeKind.Circle;
	radius: number;
}
interface Square {
	kind: ShapeKind.Square;
	sideLength: number;
}
let c: Circle = {
	kind: ShapeKind.Square,	// Error
	radius: 100
}
```

enum을 가진 타입시스템이 enum자체에 존재하는 정확한 값 집합을 알고 있다는 사실을 활용할 수 있다.
```ts
enum E {
	Foo,
	Bar
}

function f(x:E) {
	if (x !== E.Foo || x !== E.Bar) {
		// Error
	}
}
```

## Enums at runtime

`enum`은 런타임에 존재하는 실제 객체이다. 그리고 `enum` 값에서 enum 이름으로 역 매핑을 유지하는 기능이있다.
```ts
enum Enum {
	A
}
let a = Enum.A;
let nameOfA = Enum[Enum.A]	// "A"
```

위 코드는 아래와 같이 컴파일된다.
```ts
var Enum;
(function (Enum) {
	Enum[Enum["A"] = 0] = "A";
})(Enum || (Enum = {}));
var a = Enum.A;
var nameOfA = Enum[Enum.A]
```
생성된 코드에서 `enum`은 forward(`name` -> `value`) 매핑과 reverse(`value` -> `name`) 매핑을 모두 저장하는 객체로 컴파일된다.

상수 enum은 enum 키워드 앞에 `const`한정자를 사용하여 정의된다.
```ts
const enum Enum {
	A = 1,
	B = A * 2
}
```
`const enum`은 상수 `enum` Expression만 사용할 수 있고 일반 `enum`과 달리 컴파일 중에 완전히 제거된다.
```ts
const enum Directions {
	Up,
	Down,
	Left,
	Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right]
```

## Ambient enums

이미 존재하는 enum 타입의 형태를 기술하기위해 사용된다.
```ts
declare enum Enum {
	A = 1,
	B,
	C = 2
}
```
초기화되지 않은 ambient enum은 항상 계산된 것으로 간주된다.
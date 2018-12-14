# [TypeScript] Variable Declarations (변수 선언) - const / Destructuring

`const` 선언은 변수를 선언하는 또 다른 방법이다.

```ts
const numLivesForCar = 9
```
`const` 선언은 `let` 선언과 같지만 (둘 다 블록 레벨 스코프) 초기화되면 값을 변경할 수 없다.

```ts
const numLivesForCar = 9
const kitty = {
	name: "Aurora",
	numLives: numLivesForCat,
}

// Error
kitty = {
	name: "Danielle",
	numLives: numLivesForCat
}

// Okay
kitty.name = "Rory"
kitty.name: "Cat",
kitty.numLives--
```
`const` 변수의 내부 상태는 여전히 수정 가능하다.

## `let` vs `const`

최소 권한의 원칙을 적용하려면 수정하려는 모든 선언은 `const`를 사용해야 한다.
- 변수에 write 할 필요가 없는 경우
- 같은 코드를 이용하여 협업하는 사람들이 자동으로 객체에 값을 쓰지 않아야 하는 경우
- 변수에 실제로 재할당이 필요하지 않는 경우


## Array destructuring (배열 구조 분해 할당)

```ts
let input = [1, 2]
let [first, second] = input
console.log(first)		// 1
console.log(second)		// 2
```
위 코드는 `first`와 `second`라는 두 개의 새로운 변수를 만듭니다.
이는 인덱스를 이용하는 것과 동일하지만 훨씬 편리하다.

```ts
// swap
[first, second] = [second, first]
```

함수에 대한 Parameter에 사용하면 다음과 같다.
```ts
function f([first, second]: [number, number]) {
	console.log(first)
	console.log(second)
}
```

`...` 구문을 사용하여 목록의 나머지 항목에 대한 변수를 만들 수 있다.
```ts
let [first, ...rest] = [1, 2, 3, 4]
console.log(first)		// 1
console.log(rest)		// [2, 3, 4]
```

물론 이것은 JavaScript이므로 관심이 없는 나머지 요소는 무시할 수 있다.
```ts
let [first] = [1, 2, 3, 4]
console.log(first)		// 1
```

## Object destructuring (객체 구조 분해 할당)

```ts
let o = {
	a: "foo",
	b: 12,
	c: "bar"
}
let {a, b} = o
```
`o.a`와 `o.b`에서 새로운 변수 `a`와 `b`가 생성된다.
필요하지 않으면 `c`는 건너 뛸 수 있다.

배열 구조 분해 할당과 마찬가지로 선언 없이 할당할 수 있다.
```ts
({a, b} = { a: "baz", b: 101})
```
이 문장을 괄호로 묶어야 한다. JavaScript는 일반적으로 `{`를 블록의 시작으로 구문 분석한다.

`...` 구문을 사용하여 객체의 나머지 항목에 대한 변수를 만들 수 있다.

```ts
let { a, ...passthrough} = o
let total = passthrough.b + passthrough.c.length
```

### Property 재명명 (renaming)

Property에 다른 이름을 지정할 수도 있다.
```ts
let { a: newName1, b: newName2 } = o
```
```ts
let newName1 = o.a
let newName2 = o.b
console.log(newName1)		// foo
console.log(newName2)		// 12
```
타입을 지정하는 경우 전체 destructuring된 후에 타입을 지정해야 한다.
```ts
let { a, b }: { a: string, b: number } = o
console.log(a)		// foo
console.log(b)		// 12
```

## 기본값 (default value)

기본값을 사용하면 속성이 정의되지 않은 경우 기본값을 지정할 수 있다.
```ts
function keepWholeObject(wholeObject: {a: string, b?: number}) {
	let { a, b = 1001 } = wholeObject
	console.log(a, b)
}
const d = {
	a: "asdf"
}
const e = {
	a: "asdf",
	b: 123
}

keepWholeObject(d)		// asdf 1001
keepWholeObject(e)		// asdf 123
```
`keepWholeObject` 함수는 `b`가 정의되지 않았더라도 `a`와 `b`속성이 있는 `wholeObject`를 가진다.

## function 선언

Destructuring은 함수 선언에서도 작동한다.
```ts
type C = { a: string, b?: number }
function f({a, b} : C): void {}
```
하지만 Parameter의 기본값을 지정하는 것이 더 일반적이며, destructuring시 정확한 기본값을 가져오는 것은 까다로울 수 있다.
우선, 기본값 앞에 타입을 적어야 하는 것을 잊지 말아야 한다.
```ts
function f({a, b} = {a: "", b: 0}): void {}
f()		// default {a: "", b: 0}
```
그런 다음 메인 initializer가 아닌 destructured Property의 Optional Property에 대한 기본값을 지정해야 한다.
`C`에서 `b`는 Optional로 지정되었다는 것을 기억하자.
```ts
function f({a, b = 0} = {a: ""}): void {}

f({a: "yes"})	// ok deafult b = 0
f(); 		// ok
f({});		// Error
```

## Spread

Spread 연산자는 destructuring의 반대이다.
배열을 다른 배열로 펼치거나 객체를 다른 객체로 퍼뜨릴 수 있다.
```ts
let first = [1, 2]
let second = [3, 4]
let bothPlus = [0, ...first, ...second, 5]

bothPlus[1] =  100
console.log(bothPlus[1])		// 100
console.log(first[0])		// 1

first[0] =  13
console.log(bothPlus[1])		// 100
console.log(first[0])		// 13
```
위의 코드는 `bothPlus`에 `[0, 1, 2, 3, 4, 5]`값을 부여한다.
Spread는 `first`와 `second`의 얕은 복사본을 만든다.
그리고 `first`와 `second`는 Spread에 의해 값이 변하지 않는다.

객체를 Spead 할 수도 있다.
```ts
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" }
let search1 = {...defaults, food: "rich"}
let search2 = { food: "rich", ...defaults }

console.log(search1)	// { food: 'rich', price: '$$', ambiance: 'noisy' }
console.log(search2)	// { food: 'spicy', price: '$$', ambiance: 'noisy' }
```
덮어쓴다.

객체 Spread에는 몇가지 한계가 있다.
첫째, 자신의 Enumerable property(열거 가능한 속성)만 포함된다.
이는 객체의 인스턴스를 Spread할 때 메서드가 손실된다는 것을 의미한다.
```ts
class C {
	p = 12
	m() {
	}
}

let c = new C()
let clone = { ...c }
clone.p		// ok
clone.m()		// error
```

둘째, TypeScript 컴파일러는 generic function의 Parameter 변수의 Spread를 허용하지 않는다.
# [TypeScript] Variable Declarations (변수 선언) - var, let

## 변수 선언

ECMAScript6에 `let`과 `const`라는 두 개의 새로운 타입의 변수 선언자가 추가되었다.
TypeScript가 JavaScript의 상위 집합체이기 때문에 TypeScript는 당연히 `let`과 `const`를 지원한다.

## var 

전통적으로 JavaScript에서 변수를 선언하는 방
```ts
var a = 10
```

```ts
function f() {
	var a = 10
	return function g() {
		var b = a + 1
		return b
	}
}

var g = f()
g() 		// returns '11'
```

### Scope 규칙

```ts
function f(shouldInitialize: boolean) {
	if (shouldInitialize) {
		var x = 10
	}
	return x
}

f(true)		// returns '10'
f(false)		// returns 'undefined'
```
`var` 선언은 포함 함수, 모듈, 네임 스페이스 또는 전역 범위에서 액세스 할 수 잇기 때문에 `x`는 `if` 블록 내에서 선언되었지만 그 블록 외부에서 변수에 접근할 수 있었다.

```ts
function sumMatrix(matrix: number[][]) {
	var sum = 0
	for (var i = 0; i < matrix.length; i++) {
		var currentRow = matrix[i]
		for (var i = 0; i < currentRow.length; i++) {
			sum += currentRow[i]
		}
	}
	return sum
}

var list:  number[][] = [[1, 2, 3], [4, 5, 6]]

console.log(sumMatrix(list))		// 6
```
같은 변수를 여러번 선언하는 것이 오류가 아니다.

### Variable capturing quirks

```ts
for (var i = 0; i < 10; i++) {
	setTimeout(function() { console.log(i) }, 100 * i)
}
```
코드에서 `setTimeout`에 전달하는 함수 표현식은 실제로 동일한 Scope에서 같은 `i`를 참조한다.
`setTimeout`은 몇 밀리초 후에 함수를 실행하지만 for문이 실행을 멈춘후에 실행된다.
for문이 실행을 마치면 `i`의 값은 10이다. 따라서 주어진 함수가 호출될 때마다 10이 출력된다.
`setTimeout`의 내부함수는 정해진 시간 1,2,3초에 실행되는데 미리 실행된 for문 때문에 변수 `i`의 값이 바뀌게 되는 것이다.

이를 해결하는 방법은,
```ts
for (var i = 0; i < 10; i++) {
	(function(i) {
		setTimeout(function() { console.log(i)}, 100 * i)
	})(i)
}
```

## let

위의 `var`에 문제를 해결하기 위해 `let`이 도입되었다.
```ts
let hello = "Hello"
```

### Block-scoping

`let` 선언자 키워드를 사용하여 선언되면, Lexical-scope 또는 Block-scope로 불리는 Scope를 사용한다.
`var` 키워드로 선언된 변수는 자신을 포함하는 Scope 함수 외부에 노출되는 것과 달리 Block-Scope는 자신을 포함하는 가장 가까운 블록 외부 또는 for문 외부에서 액세스 할 수 없다.

```ts
function f(input: boolean) {
	let a = 100
	if (input) {
		// a를 참조할 수 있다.
		let b = a + 1
		return b
	}
	// b는 여기에 존재하지 않는다.
	return b
}
```

`catch` 절에서 선언된 변수도 비슷한 범위 지정 규칙을 가진다.
```ts
try {
	throw "oh no!"
}
catch(e) {
	console.log("Oh well.")
}

console.log(e)	// error: 'e'는 여기에 존재하지 않는다.
```

Block-scope의 특징은 실제로 선언되기 전에 읽거나 쓸 수 없다는 것이다.
변수의 선언이 모두 자신의 Temporal dead zone의 일부를 가리킬 때까지 Scope내에서 존재한다.
> Temporal dead zone
> 스코프에 들어가는 것과 접근할 수 있는 선언 사이에 기간
> ```ts
>	let aLet;
>	console.log(aLet); // undefined
>	aLet = 10;
>	console.log(aLet);
>	```

**주의 해야할 점은 Block-scope의 변수의 선언 전에 캡쳐를 할 수 있다는 것이다.**
ES2015에서는 이러한 캡처가 함수를 호출하는 시점에서 오류를 발생시키지만 지금의 TypeScript는 이러한 방식을 허용하고 있으며 에러를 발생하지 않는다.
(TypeScript는 `var`로 변환하기 때문이다.)
```ts
function foo() {
	return a
}
foo()
let a
```
< JavaScript로 변환한 후>
```js
function  foo() {
	return a;
}
foo();
var a;
```

### Re-declarations and Shadowing

`var` 선언자를 이용한 변수 선언은 횟수가 중요하지 않다.
```ts
function f(x) {
	var x
	var x
	if (true) {
		var x
	}
}
```
위의 예제에서 `x`의 모든 선언은 실제로 같은 `x`를 참조하며 이것은 완벽하게 유효하다.
하지만 `let` 선언자를 이용한 변수 선언은 이러한 방식을 허용하지 않는다.
```ts
let x = 10
let x = 11 // error
```

Block-scope 변수가 아니어도 변수의 재선언시 TypeScript에서 문제가 있음을 알려준다.
```ts
function f(x) {
	let x= 100	// error : parameter 변수에 간섭하고 있다.
}

function g() {
	let x = 100
	let x = 100	// eroor: 변수 x를 두개 선언할 수 없다.
}
```

하지만 Block-scope 변수가 Function-scope에 절대로 선언될 수 없다는 말은 아니다.
블록 범위 변수는 뚜렷하게 다른 블록 내에서 선언되어야만 한다.

```ts
function f(condition, x) {
	if (condition) {
		let x = 100
		return x
	}
	return x
}

f(false, 0)		// return '0'
f(true, 0)		// return '100'
```

**중첩된 Scope에 기존의 변수 이름을 사용하는 것을 Shadow라고 한다.**
```ts
function sumMatrix(matrix: number[][]) {
	let sum = 0
	for (let i = 0; i < matrix.length; i++) {
		// 위의 For 문에서 i랑
		var currentRow = matrix[i]
		for (let i = 0; i < currentRow.length; i++) {
		// 여기서의 i는 다른 변수이다.
			sum += currentRow[i]
		}
	}
	return sum
}
```
위 코드에서 for문은 실제로 내부 for문의 `i`가 외부 for문의 `i`를 `shadow`하기 때문에 올바르게 실행된다.
일부 코드에서 `shadow`가 필요할 수도 있지만 일반적으로 더 명확한 코드를 작성하기 위해 `shadow`는 피해야 한다.

### Block-scoped variable capturing

Scope가 실행될 때마다 변수의 "environment"를 생성한다.
Scope 내의 모든 것이 실행을 마친 후에도 "environment" 캡쳐된 변수가 존재할 수 있다.
```ts
function theCityThatalwaysSleeps() {
	let getCity
	if (true) {
		let city = "Seattle"
		getCity = function() {
			return city
		}
	}
	return getCity()
}
```
"environment"안에서 `city`를 캡처했으므로 'if'블록이 실행을 완료했음에도 불구하고 여전히 액세스할 수 있다.

이전의 `setTimeout` 예제에서는 `for`문이 반복될 때마다 변수의 상태를 캡쳐하기 위해 즉시 실행 함수를 사용해야 할 필요가 있었따.
실제로 우리가 수행한 작업은 캡쳐된 변수에 대한 새로운 변수 환경을 만드는 것이다.
이러한 방식은 TypeScript에서는 다시할 필요가 없다.

`let` 선언문은 루프의 일부로 선언될 때 크게 다르게 행동한다.
루프 자체에 새로운 환경을 도입하기 보다는 반복될 때마다 새로운 Scope를 만든다.
어쨌든 즉시 실행 함수를 사용하여 이 작업을 수행했던 이전의 `setTimeout` 예제를 `let`을 이용하여 변경할 수 있다.

```ts
for (let i = 0; i < 10; i++) {
	setTimeout(function() { console.log(i) }, 100  * i)
}
```
< JavaScript로 변환한 후>
```js
// js
var _loop_1 = function (i) {
	setTimeout(function () { console.log('a', i); }, 100 * i);
};

for (var i = 0; i < 10; i++) {
	_loop_1(i);
}
```

# [TypeScript] Basic Type

TypeScript에서는 JavaScript에서 기대할 수 있는 것과 거의 동일한 유형을 지원하며, 이를 위해 쉽게 열거할 수 있는 유형이 포함되어 있다.

## Boolean

TypeScript / JavaScript 에서의 boolean 값은 true / false 이다.
```ts
let isDone: boolean = false;
```

## Number

TypeScript의 모든 숫자는 부동 소수점 값이다.
16진수, 10진수, 리터럴, 2진수, 8진수를 지원한다.
```ts
let decimal: number = 6;
let hex: number = 0x00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

## String

텍스트 데이터 유형을 참조한다.
TypeScript에서는 JavaScript와 마찬가지로 문자열 데이터를 둘러싸기 위해 큰 따옴표("")또는 작은 따옴표 ('')를 사용한다.
```ts
let color: string = "blue";
color = 'red';
```
```ts
let fullName: string = "Bob";
let age: number = 37;
let sentence: string = `Hello, my name in ${fullName}. 
I'll be ${age + 1} yaers old next month.`;
```
에 둘러싸여 있으며 ${ expr } 형식의 표현식을 사용할 수 있다.

## Array

TypeScript는 JavaScript와 마찬가지로 값 배열을 사용할 수 있다.

1. element의 유형을 적은 후 []을 사용하여 배열임을 나타낸다.
```ts
let list: number[] = [1, 2, 3];
```
2. 일반 Array 유형 사용
```ts
let list:Array<number> = [1, 2, 3];
```

## Tuple

고정된 수의 element 유형을 정하고 그 유형이 동일할 필요는 없다.
```ts
let x: [string, number];
x = ["hello", 10];
y = [10, "hello"];
```
```ts
console.log(x[0].substr(1)); // o
console.log(x[1].substr(1)); // x
```
인덱스가 있는 요소에 접근할 때 올바른 유형이 검색된다.

```ts
// 2.7 이하의 버전에서는 배열의 요소가 튜플 타입에 선언된 개수를 초과하면 유니언 타입을 적용받았지만 지금은 아니다.
x[3] = "world";
console.log(x[5].toString());
x[6] = true;
```

## Enum

JavaScript에서는 enum이 없었다.
Enum은 숫자 값 집합에 이름을 부여하는 방법이다.

```ts
enum Color {Red, Green, Blue}
let c: Color = Color.Green

console.log(c)		// 1
console.log(typeof c)		// number
console.log(typeof Color)		// object
```
기본적으로 enum은 0번부터 번호를 매긴다.
구성원 중 하나의 값을 수동으로 변경할 수 있다.
```ts
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green

console.log(c)		// 2
```
열거형의 모든 값을 수동으로 설정할 수 있다.
```ts
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green
// 숫자와 문자열만 해당된다.

console.log(c)		// 2
```

enum의 특징 중 하나는 숫자 값에서 enum 값의 이름으로 이동 할 수 있다는 것이다.
```ts
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

console.log(colorName);		// Green
```

리버스 매핑을 하기 때문이다.
```ts
console.log(JSON.stringify(Color));
// 결과 값: {"1": "Red", "2": "Green", "3": "Blue", "Red": "1", "Green": "2", "Blue" : "3"}
```


## Any

프로그램을 작성할 때 알지 못하는 변수 유형을 설명해야 할 때 컴파일 시 타입 검사를 하지 않고 지나가도록 해야한다.
이 때 `any` 타입을 사용한다.
```ts
let notSure: any = 4
notSure = "maybe a string instead"
notSure = false
```

`any` 타입은 기존 JavaScript와 같이 작업하는 강력한 방법 중 하나이다.
컴파일 하는 동안 옵트 인하거나 옵트아웃 할 수 있다.
JavaScript의 Object 타입과 비슷한 역할을 하지만 `object` 타입의 변수는 값을 할당만 할 수 있다.
any는 런타임시에 속성의 유무를 검사하고 object는 컴파일 시에 속성의 유무를 검사한다.
실제로 존재하는 메소드라도 임의의 메소드를 호출할 수는 없다.

```ts
let notSure: any = 4
notSure.ifItExists()		// js로 변환해서 실행하면 에러남
notSure.toFixed()

let prettySure: Object = 4
prettySure.toFixed()		// ts에서 실행하면 에러
```

배열에 있는 다른 타입이 혼합되어 있을 수 있다.
```ts
let list: any[] = [1, true, "free"]
list[1] = 100
```

### noImplicitAny 옵션

타입 선언을 생략했을 때 암시적으로 any 타입이된다.
any 타입의 사용을 강제하려면 컴파일러 옵션 중 noImplicitAny를 true로 설정하면 된다.
```
// tsconfig.json
{
	"compilerOptions": {
		"noImplicitAny": true
	}
}
```

`noImplicitAny` 옵션은 `false`가 기본값이므로 이 옵션을 생략한다면 any타입임을 생략해도 괜찮다.
옵션이 true일 때 함수의 매개변수에서 any타입을 선언하지 않으면 컴파일 오류가 발생한다.



## Void

`void`는 타입이 전혀 없다. (`any`와 반대)
일반적으로 값을 반환하지 않는 함수의 반환 유형으로 사용한다.
```ts
function warnUser(): void {
	alert("This is my warning message")
}
```

`void` 타입의 변수 선언은 `undefined` 또는 `null`만 할당할 수 있다.
```ts
let nuusable: void = undefined
```

## Null and Undefined

TypeScript 에서 `undefined`와 `null`은 실제로 각각 이름의 타입을 가지고 있다.

```ts
let u: undefined = undefined
let n: null = null
```
기본적으로 `null` 과 `undefined`는 다른 모든 유형의 하위 유형이다.
즉, `null`과 `undefined`를 `number`와 같은 것에 할당할 수 있다. (???)
```ts
let a:  object  =  null		// 에러뜸
```

`--strictNullChecks` 플래그를 사용할 경우 `null`과 `undefined`는 `void`와 각각의 타입 변수에만 할당 가능하다.
이렇게 하면 많은 일반적인 오류를 피할 수 있다.
`string` 또는 `null` 또는 `undefined`를 전달하고자 하는 경우, union 타입 `string`|`null`|`undefined`을 사용할 수 있다.

## Never

절대 발생하지 않는 값의 타입을 나타낸다.

`never`은 함수 표현식의 리턴 타입이거나 리턴하지 않는 표현식이다.
`never` 타입은 모든 타입의 서브 타입이다. 모든 타입에 assign 가능하다.
어떤 타입도 `never`에 assign 되지 않는다.
```ts
function error(message: string): never {
	throw new Error(message)
}

function fail() {
	return error("Something failed")
}

function infiniteLoop(): never {
	while(true) {}
}
```
## Object

`Object`는 non-primitive 타입이다. `number`, `string`, `boolean`, `symbol`, `null`, `undefined` 모든 것을 나타내는 유형이다.

```ts
declare function create(o: object | null): void

create({ prop: 0 })
create(null)

create(42)
create("string")
create(false)
create(undefined)
```

## Type assertions

어떤 값에 대해 프로그래머는 때때로 TypeScript보다 더 많은 정보를 알 수 있다.
보통 이런 경우는 어떤 독립체의 타입이 현재 타입보다 구체적인 타입을 알고 있을 때이다.

Type assertion은 다른 언어의 타입 변환가 비슷하지만 특별한 검사나 데이터 재구성을 수행하지 않는다.
런타입에 영향을 미치지 않으며 컴파일러에서만 사용된다.

Type assertion에는 두 가지 형식이 있다.

1. Angle-bracket(<>) 구문
```ts
let someValue: any = "This is a string"
let strLEngth: number = (<string>someValue).length
```
2. as 구문
```ts
let someValue: any = "this is a string"
let strLength: number = (someValue as string).length
```

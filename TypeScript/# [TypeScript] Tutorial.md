# [TypeScript] Tutorial

## 설치하기

TypeScript 툴을 사용하기 위한 두 가지 방법
1. Via npm (Node.js 패키지 관리자)
2. TypeScript의 Visual Studio 플러그인 설치

```
// npm으로 설치하기
npm install -g typescript
```

## TypeScript 파일 만들기

`greeter.ts`파일에 코드 입력하기

```ts
function greeter(person) {
	return "Hello" + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```

## 코드 컴파일하기

`.ts` 확장자를 사용했지만 이 코드는 단지 자바스크립트이다.
기존 JavaScript 앱에서 바로 복사 붙여넣기가 가능하다.

커맨드 라인에서 TypeScript 컴파일러 실행
```
tsc greeter.ts
```
결과는 greeter.js 파일이 될 것이다.
JavaScript에서 TypeScript를 사용하여 실행중이다.

## 타입 어노테이션 (주석 입력)

TypeScript의 타입 어노테이션은 함수 또는 변수의 의도된 계약을 기록하기 위한 간단한 방법이다.

```ts
function grreter(person: string) {
	return "Hello, " + person;
}

let user = [0, 1, 2];

document.body.innerHTML = greeter(user);
```
단일 문자열 파라미터와 함께 greeter 함수를 호출하였고, 배열을 전달하였다.
< 에러 >
```
error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'string'.
```
TypeScript를 사용하면 예기치 않은 수의 매개변수를 사용하여 이 기능을 호출한다.

오류가 있지만 greeter.js 파일은 생성된다.
코드에 오류가 있더라도 TypeScript를 사용할 수 있다. (하지만 제대로 실행되지 않음)


## 인터페이스

```ts
interface Person {
	firstName: string;
	lastName: string;
}

function greeter(person: Person) {
	return "Hello, " + person.firstName + " " + person.lastName;
}

let user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```
`firstName`, `lastName` 필드가 있는 인터페이스를 사용한다.
TypeScript에서는 내부구조가 호환되면 두 가지 유형(Person, user)이 호환된다.

## 클래스

TypeScript는 클래스 기반 객체 지향 프로그래밍같은 JavaScript의 새로운 기능을 지원한다.

```ts
// 생성자와 몇 개의 public 필드가 있는 클래스를 만든다.
class Student {
	fullName: string;
	constructor(public firstName: string, public middleInitial: string, public lastName: string) {
		this.fullName = firstName + " " + middleInitial + " " + lastName;
	}
}

interface Person {
	firstName: string;
	lastName: string;
}

function greeter(person: Person) {
	return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```
생성자에 public 인수를 사용하는 것은 자동으로 이름으로 속성을 만들 수 있게 해준다.

`tsc greeter.ts`를 다시 실행하면 생성된 JavaScript가 이전 코드와 동일하다.
TypeScript의 클래스는 JavaScript에서 자주 사용되는 같은 프로토타입 기반이다.

## TypeScript 웹 앱 실행

```html
<!DOCTYPE html>
<html>
	<head><title>TYpeScript Greeter</title></head>
	<body>
		<script src = "greeter.js"></script>
	</body>
</html>
```
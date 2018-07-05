
# [JavaScript] basic 2
> 인사이드 자바스크립트
### 1. 자바스크립트 데이터 타입
#### 1-1. 기본 타입 : 숫자(number), 문자열(string), 불린값(boolean), null, undefined
```javascript
var intNum = 10;
var emptyVar;
var nullVar = null;
console.log(typeof intNum, typeof emptyVar, typeof nullVar);
// 결과: number undefined object
```

#### 1-1-1. 숫자
모든 숫자를 실수로 처리한다.
```javascript
var num = 5 / 2;
console.log(num); // 결과: 2.5
console.log(Math.floor(num)); // 결과: 2
/* Math.floor : 소수 부분을 버린 정수부분만 출력 */
```
#### 1-1-2. 문자열
작은 따옴표(' ')나 큰 따옴표(" ")로 생성한다.
C언어에서 *char* 타입과 같이 문자 하나만 나타내는 타입은 없다.
한 번 정의된 문자열은 변하지 않는다.
```javascript
var string = "hello";
console.log(string[0]); // 결과: h
string[0] = "k";
console.log(string); // 결과: hello 
```
#### 1-1-3. 불린값
true와 false값을 나타낸다.

#### 1-1-4. null과 undefined
두 타입 모두 **'값이 비어있음'** 을 나타낸다. 
**undefined** : 기본적으로 값이 할당되지 않은 변수이다.
undefined 타입 변수는 변수 자체의 값 또한 undefined이다.
**null**: 명시적으로 값이 비어있음을 나타내는데 사용한다.
```javascript
var nullVar = null;
console.log(typeof nullVar === null); // 결과: false
console.log(nullVar === null) // 결과: true 
/* null타입은 typeof결과가 object이다. */
```

#### 1-2. 참조 타입 : 객체(Object)타입
객체는 단순히 **'이름(key) : 값(value)'** 형태의 프로퍼티들을 저장하는 컨테이너이다.
기본 타입은 하나의 값만 가지지만 객체 타입은 여러개의 프로퍼티들을 포함할 수 있다.
객체의 프로퍼티는 함수로 포함할 수 있으며 자바스크립트에서는 이러한 프로퍼티를 메서드라고 부른다.

1) 배열 (array)
2) 함수 (function)
3) 정규표현식

#### 1-2-1. 객체를 생성하는 방법
자바스크립트는 클래스라는 개념이 없고, **객체의 리터럴**이나 **생성자 함수**등 별도의 생성 방식이 존재한다.

#### 1-2-1.1 Object() 생성자 함수 이용
```javascript
// Object()를 이용하여 빈 객체 생성
var foo = new Object();

// 객체의 프로퍼티 생성
foo.name = 'foo';
foo.age = 30;

console.log(typeof foo); // 결과 : object
console.log(foo); // 결과 : { name: 'foo', age: 30 }
```

#### 1-2-1.2 객체 리터럴(= 표기법) 방식 이용
 표기법만으로도 객체를 생성할 수 있다.
 중괄호({})를 사용하여 객체를 생성한다. 
 {}에 아무것도 적지 않으면 빈 객체가 생성된다.
 중괄호 안에 ```'프로퍼티 이름' : '프로퍼티 값'```을 적어준다.
 ```'프로퍼티 이름'``` 은 문자열, 숫자가 올 수 있다.
 ```'프로퍼티 값'```은 값을 나타내는 어떠한 표현식도 올 수 있다. 함수일 경우 *메소드*라고 부른다.
```javascript
var foo = {
	name: 'foo',
	age: 42
}

console.log(typeof foo); // 결과: object
console.log(foo); // 결과: { name: 'foo', age: 30 }
```

#### 1-2-2. 객체 프로퍼티 읽기/쓰기/갱신
객체 프로퍼티에 접근하는 방법

- 대괄호([]) 표기법
-  마침표(.) 표기법

```javascript
var foo = {
	name: 'foo';
	age: 42
}
```

#### 1-2-2.1 객체 프로퍼티 읽는 법
```javascript
console.log(foo.name); // 결과 : foo
console.log(foo['name']); //결과 : foo
console.log(foo[name]); // 결과 : undefined
console.log(foo.major) // 결과 : undefined
```
마침표 표기법은 객체 다음에 마침표를 직고 원하는 속성값을 찍는다.
대괄호 표기법은 접근하려는 객체의 프로퍼티 이름을 문자열 형태로 만들어야 한다.



#### 1-2-2.2객체 프로퍼티 갱신하는 법
```javascript
foo.name = 'goo';
console.log(foo.name); // 결과 : goo
foo['name'] = 'hoo';
console.log(foo.name); // 결과 : hoo
```
프로퍼티에 접근해서 프로퍼티 값을 갱신할 수 있다.

#### 1-2-2.3객체 프로퍼티 동적 생성하는 법
```javascript
foo.major = 'media';
console.log(foo.major); // 결과 : media
```
자바스크립트 객체의 프로퍼티에 값을 할당할 때 프로퍼티가 이미 있을 경우에는 해당 프로퍼티 값이 갱신되고 없을 경우 프로퍼티가 새로 생성된 후 값이 할당된다.

#### 1-2-2.4 대괄호 표기법을 사용해야 하는 경우
```javascript
foo['full-name'] = 'foo bar';
console.log(foo['full-name']); // 결과 : foo bar
console.log(foo.full-name); // 결과 : NaN
```
접근하려는 프로퍼티가 표현식이거나 예약어일 경우 대괄포 표기법만 사용해야 한다.
프로퍼티가 'full-name'이다. 이 때 '-' 연산자가 있는 표현식이다.

**NaN (Not a Number) 이란?**
수치 연산을 해서 정상적인 값을 얻지 못할 때 출력되는 값이다.
```javascript
console.log(1-'hello'); // 결과 : NaN
```
```foo.full-name```에서 ```-``` 로 인해 ```(foo.full) - (name)```으로 수치연산을 했기 때문에 NaN값이 나온다.

#### 1-2-3. for in 문과 프로퍼티 출력
객체에 포함된 모든 프로퍼티에 대해 루프 수행이 가능하다.
```javascript
var foo = {
	name: 'foo',
	age: 42
}

var prop;
for (prop in foo) {
	console.log(prop, foo[prop]);
}
/* 
	결과: name foo
		age	 42
*/
for (prop in foo) {
	console.log(prop, foo.prop);
}
/* 
	결과: name undefined
		 age  undefined
*/
```
prop 변수에 foo 객체의 프로퍼티가 할당된다. 할당된 프로퍼티 이름을 이용하여 **대괄호 표기법**을 이용하여 프로퍼티 값을 출력한다.

#### 1-2-4. 객체 프로퍼티 삭제
```delete``` 연산자를 이용해 즉시 삭제 할 수 있다. 
**객체의 프로퍼티를 삭제하지만 객체 자체를 삭제하지는 못한다.**

```javascript
var foo = {
	name: 'foo',
	age: 42
}

delete foo.age;
console.log(foo.age); // 결과 : undefined

delete foo;
console.log(foo.name); // 결과 : foo
```
첫번째의 경우, 'age' 프로퍼티가 삭제되었기때문에 존재 하지 않는 프로퍼티에 접근하여 undefined값이 출력된다.
두번째의 경우, foo객체가 삭제되지 않았다. foo는 프로퍼티가 아닌 객체이기 때문이다.
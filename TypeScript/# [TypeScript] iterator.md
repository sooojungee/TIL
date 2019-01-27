# [TypeScript] Iterator

## Iterables

- 객체가 `Symbol.iterator` 프로퍼티에 대한 구현을 가지고 있다면 *Iterable*로 간주된다.

- `Array`, `Map`, `Set`, `String`, `Int32Array`, `Uint32Array` 등과 같은 내장 타입은 이미 구현된 `Symbol.iterator` 프로퍼티를 가지고 있으므로 iterable하다고 할 수 있다.

- 객체의 `Symbol.iterator` 함수는 반복할 값 목록을 반환한다.

## `for .. of` 명령문

`for ..of`는 Iterable 객체를 반복하며, 객체의 `Symbol.iterator` 프로퍼티를 호출한다.

- 배열
	`for (const i of array)`
- 객체
	`for (const key of Object.keys(obj))`

```ts
const array = ['first', 'second', 1];
const obj = {
	name: 'Mark',
	age: 32
}

// 배열만 가능
for (const item of array) {
	console.log(`${typeof item}, ${item}`);
}


// error
// for(const item of obj) {
// 	  console.log(`${typeof item}, ${item}`);
// }

// 객체
for(const prop of Object.keys(obj)) {
	console.log(`${typeof prop}, ${prop}`)
}
```
```
// 결과
string, first
string, second
number, 1
string, name
string, age
```

## `for ..in` 명령문

반복되는 객체의 `key` 목록을 반환한다.
어떤 객체에서도 작동한다.
index가 무조건 string으로 나온다.

```ts
const array = ['first', 'second'];
const obj = {
	name: 'Mark',
	age: 32
}

for (const item in array) {
	console.log(`${typeof item} + ${item}`);
}

for(const prop in Object.keys(obj)) {
	console.log(`${typeof prop}, ${prop}`)
}
```
```
// 결과
string, 0
string, 1
string, 2
string, name
string, age
```

## `for ..of` vs `for ..in`

공통점: 둘 다 리스트를 반복한다.

### 반환

- `for in`은 반복되는 객체의 `key` 목록을 반환한다.
- `for of`는 반복되는 객체의 숫자 프로퍼티 값 목록을 반환한다.
```ts
let list = [4, 5, 6];

for (let i in list) {
	console.log(i); 	// "0", "1", "2"
}

for (let i of list) {
	console.log(i); 	// "4", "5", "6"
}
```

### 사용

- `for in`은 객체의 프로퍼티를 검사하는 방법으로 사용된다.
- `for of`는 반복 가능한 객체의 값을 아는데 사용된다.

`Map`과 `Set`같은 내장 객체는 저장된 값에 접근할 수 있는 `Symbol.iterator` 속성이 있다.






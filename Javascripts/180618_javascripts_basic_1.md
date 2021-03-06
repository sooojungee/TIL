# [javascript]


## 1. console 사용하기
 
- 디버깅시에 로그파일에 관련된 정보를 기록하면 디버깅이 훨씬 쉬워진다.
> 디버깅이란? 버그를 찾아서 수정하거나 에러를 피해나가는 처리과정
- console의 메소드는 약 10개가 있다.
- 실제로는 10여개의 메소드가 존재하지만 모든 브라우저가 제공하지는 않기 때문에 확인한 후 사용해야한다.
### 1-1. console.log
```console.log(message)```
콘솔창을 통해 로그를 찍는다.
 개체를 전달하는 경우, 브라우저의 개발자 메뉴의 console창에서 message를 확인할 수 있다.
```
console.log('hello');
console.log('hello' + text);
```
### 1-1-1. alert() 
```
alert('alert~!');
```
alert 대화상자를 표시한다.
이전의 자바스크립트 디버깅 방식은 alert()를 사용하는 것이었다.

### 1-1-2. alert()과 console.log의 차이

1) console.log는 정상적인 프로세스를 방해하지 않는다.
alert()창을 사용할 경우, 중간에 멈추는 현상이 발생한다.
 2) 반복문과 같은 경우 alert()를 사용하면 일일히 엔터를 입력해야하는 불편함이 있다.
 console.log를 사용하면 이러한 불편함을 줄일 수 있다.
 3) alert는 어떠한 웹 브라우저에서도 이용할 수 있다.
 console.log는 모든 웹 브라우저가 제공하지 않는다.
 4) console.log가 보다 상세한 데이터를 표시하기 용이하다.


### 1-2. console.warn
```
console.warn(message)
```
경고 기호(느낌표)와 message를 콘솔창에 보낸다.


```
console.warn('warn!');
```

### 1-3. console.error
```
console.error(message)
```
console창에 message를 보낸다.
오류 기호(x)와 빨간색 메세지를 띄운다.
```
console.error('error!');
```

### 1-4. console.clear
```
console.clear()
```
콘솔창의 로그 내용을 모두 지운다.

## 2. 변수 선언

변수는 **var**로 선언한다.
변수의 초기값은 **undefined**
자바스크립트에는 명시적인 타입이 없다.
자바스크립트에는 블록 유효범위가 없다.
> 자바와 같은 언어와 달리 블록({})안에서 선언된 변수는 블록이 닫힌 이후에도 접근이 가능하다.
```

var a = 10; // 정수형
var b = 'a'; // string
var c = null;
var d = undefined;
var e = [1, 2, 3]; //배열
var f = {1: 10}; //객체
```

자바스크립트는 한 변수를 다른 타입의 값으로 할당할 수 있다.
```
/* g는 배열이다. 
배열 안에는 정수, 실수, 문자, 객체가 들어있다.
*/
var g = [1, 2.9, 'a', {a: [2, 3, 4]}];
```

## 3. 함수 선언

### 3-1. 함수 선언식
일반적인 프로그래밍 언어에서의 함수 선언과 비슷한 형식이다.
구현한 위치와 관계 없이 브라우저가 자바스크립트를 해석할 때 맨 위로 끌어 올린다. **(호이스팅)**
```
function h(){
	//함수 내용
}
```


### 3-2. 함수 표현식
자바스크립트 언어의 특징을 활용한 방식
```
var h = function() {
	console.log('이것은 함수');
};
```


### 3-3. 함수 호출
```
h();
```

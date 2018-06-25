# [JavaScript] Module Pattern (모듈 패턴)

**모듈**이란 전체 코드의 일부를 독립된 코드로 분리해서 만든 것이다.

JavaScript에서 모듈은 '클래스'라고 할 수 있다.
모듈 패턴은 클래스의 캡슐화 특성을 가진 패턴이다.
상태 및 동작을 다른 클래스에서 접근하지 못하게 한다.
즉, **모듈 패턴은 public , private 접근 권한을 설정한다.**
(자바스크립트에서는 public, private와 같은 접근 제한자가 없다.)

## 모듈을 구현하는 방법

### 1. IIFE(Immediately-Invoked-function-Expressions) 즉시 호출 함수 표현식
첫번째 괄호()로 둘러싸인 익명함수를 사용함으로 내부 변수에 접근하지 못하도록 막을 수 있다.
두번째 괄호()를 사용함으로써 함수를 즉시 해석해서 실행한다.

```javascript
(function() {
	// 변수와 함수들이 private로 선언됨.
})();
```

IIFE 를 변수에 할당하면 IIFE자체는 저장되지 않고 함수 실행 결과만 저장된다.

```javascript
var test = (function() {
	var x = 1;
	return x;
})(); //즉시 실행
test(); // 결과: 1
```

### 2. 객체 리터럴을 이용
객체 리터럴은 독립된 모듈이다.
단 하나의 객체를 만들 때 사용한다.
독립된 모듈에는 필요한 내부 변수와 내부 함수를 모두 가지고 있어야 한다.

```javascript
var myModule = {
	x : 'hello'
	y : function(){
		console.log(x);
	}
};
```

### 3. 클로저(closure)을 이용
클로저를 사용해서 상태와 구조를 캡슐화시킬 수 있다.
내부 변수와 내부 함수를 가지고 있는 객체를 생성할 때는 클로저를 이용해야 한다.
필요한 부분만 외부로 노출시켜 다른 부분에서 사용할 수 있게 할 수 있다.
```javascript
var myModule = (function() {

	//private
	var i = 0;

	return{
	// public
		countup: function(){
			return i++;
		},	
	
		countdown: function(){
			return i--;
		}
	
		setI : function(x){
			i = x;
		}
		
		getI : function(){
			console.log(i);
		}
	}
})();

myModule.countup();
myModule.countdown();
//countup, countdown으로 직접 접근은 불가능
// myModule을 통해서만 접근 가능

```

## 모듈패턴의 장단점
- 장점
1. 캡슐화가 가능하다.
- 단점
1. public과 private를 다른 방식으로 접근해야한다.
 2. private 멤버들 버그처리가 복잡해진다.

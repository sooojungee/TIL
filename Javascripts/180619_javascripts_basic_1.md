## 4. jquery 함수 호출

### 4-1. 변수 선언

1. **$** 을 사용하여 변수를 선언한다.
2. jQuery에서 사용하는 내장 함수들(.css, show() 등)을 모두 이용할 수 있다.
3. $는 jQuery 랩 객체에 대한 일반적인 참조이므로 jQuery가 래핑된 변수를 쉽게 읽을 수 있다.
4. $를 사용한 변수는 해당 변수가 jquery 변수임을 의미한다.

```
var $text = $('.text');
```


### 4-2. 함수 호출

```
console.log($)
```
tag, class, attribute, id를 호출 할 수 있다.


```
/* class 호출 */
console.log($('a.login'));
```
```
/* id로 호출 */
console.log($('#loginText'));
똑같은 클래스가 여러개일 경우 ID로 구분하는데,
자바 스크립트를 쓸 경우, class명은 겹칠 수 있기 때문에 id를 사용한다. 
```

### 4-3. selector $(선택자)

css선택자를 그대로 사용할 수 있다.

선택자를 사용하여 각 요소들을 쉽고 빠르게 접근할 수 있다.

tag로 가져오기
```
var text = $('div')
//모든 div태그가 선택된다.
```


 id로 가져오기
```
var text = $('#inputId');
//하나만 선택된다.
```

class로 가져오기
```
var text = $('.input');
//중복이 가능하기 때문에 여러개를 선택할 수 있다.
```

attribute로 가져오기
```var text = $('input[name=my_name]');```

## 5. 함수 사용하기

### 5-1. 익명함수로 사용
함수의 이름은 선언하지 않고 사용하는 방식이다.
반드시 함수를 호출하는 코드보다 먼저 작성되어야 한다.
```
$input.on('click', function(){
	//함수내용 적기
});
```
익명 함수를 사용할 경우, 함수를 재활용 할 수 없다는 단점이 있다.

### 5-2.  메소드로 나눈 후 함수로 사용
```
var sendMessage = function(){
};

$input.on(click, sendMessage);
```
함수의 재활용이 가능하게 만든다.

**주의할 점**
sendMessage 는 등록,
sendMesage ()는 선언이다.

# [TypeScript] Class 

객체지향 프로그래밍 요소 | 자바스크립트 | 타입스크립트
------|---------|----------
클래스 | class | class
인터페이스  | 지원 안함 | interface
인터페이스 구현 | 지원 안함 | implements
상속 | extends | extends
생성자 | constructor(){} | constructor(){}
접근 제한자 | 지원 안함 | private, public, protected
final 제한자 | 지원 안함 | readOnly (TS 2.0부터 지원)
static 키워드 | static | static
super 키워드 | super | super

## 클래스 선언과 객체 생성

```ts
class Rectangle {
	x: number;
	y: number;

	constructor(x: number, y: number) {
		this.x = x;
		this.y = y;
	}
	
	getArea(): number {
		return this.x * this.y
	}
}
```
이렇게 선언한 `Rectangle`클래스는 클래스 타입이 된다.

< JavaScript에서 표현되는 방식 >
```ts
var Rectangle = (function() {
	function Rectangle(x, y) {
		this.x = x
		this.y = y	
	}
	Rectangle.prototype.getArea = function() {
		return this.x + this.y
	}
	return Rectangle
}());
```
생성자 함수는 다음과 같은 형태이다.
`(function() { ... }());`
생성자 함수에 클로저를 이용해 비공개된 내부 메서드를 캡슐화하는데, 전역 이름 공간을 더럽히지 않는다는 장점이 있다.

`Rectangle` 클래스 타입은 다음의 인터페이스 타입과 정확히 일치한다.
```ts
interface Rectangle {
	x: number
	y: number
	getArea(): number
}
```

클래스 내부에는 생성자인 `contstructor`을 정의한다.
생성자는 객체를 생성할 때 클래스에 필요한 설정을 매개변수로 전달받아 멤버 변수를 초기화한다.
생성자를 생략하면 기본 생성자 (default constructor)를 호출한다.
```ts
class Rectangle { }
```

### 객체 생성

클래스는 멤버 변수와 멤버 메서드 등으로 구성된 틀이며 클래스를 실제로 사용하려면 객체로 생성해야한다.
```ts
let rectangle = new Rectangle(1, 5);
// rectangle: 객체 참조 변수이고 객체를 참조하므로 인스턴스이다.
// new Rectangle(1, 5): Rectangle 객체를 생성
```
위 코드에서 보면 new 키워드를 이용해 Rectangle 객체를 생성해 객체 참조 변수에 할당했다.
생성된 객체는 실제 메모리에 위치하고 객체의 참조가 객체 참조 변수에 할당된다. => 인스턴스화 (instantiate)

## 상속 관계와 포함 관계

상속 관계 : is-a ( 자식클래스가 부모 클래스에 선언된 멤버 변수나 멤버 메서드를 상속받아 사용하는 것)
포함 관계: has-a (한 클래스에 다른 클래스를 멤버 변수로 선언하는 것)

### 1. 상속 관계
상속은 계층을 만들어 코드의 중복을 줄이는 객체지향 프로그래밍 방법이다.
자식 클래스는 부모 클래스에 공개된 메서드나 변수를 상속 받아 **IS-A** 관계가 성립된다.
`class 공룡 extends 동물`
'공룡은 동물이다' => o 상속 관계가 성립된다.

<형식>
```ts
class dinosaur extends animal {
	constructor() {
		super()
	}
}
```
타입스크립트는 클래스에 대해 단일 상속만 지원하므로 
**자식 클래스는 하나의 부모 클래스만 상속 받을 수 있다.**
자식 클래스가 부모 클래스를 상속받을 때는 자식 클래스의 생성자에 `super()`메서드를 호출해 부모 클래스의 생성자를 호출해줘야 한다.

### 2. 포함 관계
포함 관계는 클래스가 다른 클래스를 포함하는 **HAS-A** 관계이다.
1. 합성(composition) 관계
합성 관계는 전체가 부분을 포함하며 강한 관계이다.
```ts
class Engine { }
class Car {
	private engine
	constructor() {
		this.engine = new Engine()
	}
}

let myCar = new Car()
myCar = null
```
Car 클래스에 선언된 engine 객체는 Car객체(myCar)가 new로 생성되고, 객체가 Null이 되면 함께 제거된다. 

2. 집합(aggregation) 관계
집합 관계는 전체가 부분을 포함하며 약한 관계이다.
```ts
class Engine { }
class Car {
	private engine: Engine
	constructor(engine: Engine) {
		this.engine = engine
	}
}

let engine = new Engine()
let car = new Car(Engine)
```
Car 클래스의 car 객체가 생성될 때 외부에서 생성된 `engine`객체를 전달하고 있다. 따라서, car 객체에 null이 할당돼 제거되더라도 `engine`객체는 Car 클래스 외부에 선언되 있으므로 제거되지 않는다.

### 상속 관계와 포함 관계를 모두 고려해 구현하기

부모 클래스에는 공통적인 기능에 해당하는 일반적인 메서드를 구현한다.
자식 클래스에는 부모 클래스에서 구현하지 못한 세부적인 메서드를 추가해 구현한다.

```ts
class Flashlight {
	constructor(public lightIntensity) { }
}

class Bicycle {
	constructor(public numberOfWheel: number) { }

	getNumberOfWheel(): number {
		return this.numberOfWheel
	}
}

class MountainBike extends Bicycle {
	flashlight: Flashlight

	constructor(public numberOfWheel: number, public hasBackSaddle: boolean) {
		super(numberOfWheel)
		this.flashlight = new Flashlight(90)
	}
	getHasBackSaddle() {
		return this.hasBackSaddle
	}
	getNumberOfWheel() {
		return this.numberOfWheel
	}
}

let hasBackSaddle = true
let numberOfWheel = 2
let mountainBike = new MountainBike(numberOfWheel, hasBackSaddle)
console.log(mountainBike.getHasBackSaddle())	// true
console.log(mountainBike.getNumberOfWheel())	// 2
```
`MountainBike` 클래스는 extends 키워드를 이용해 `Bicycle` 클래스를 상속받는다.
산악용 자전거가 손전등(flashlight)를 포함하고 있는 포함 관계를 나타낸다.

## 접근 제한자의 사용법

자바스크립트 ES6에서는 접근 제한자를 제공하지 않는다.
타입스크립트에서는 제공한다.

접근제한자 | 특징 | 상속여부 | 외부 객체를 통한 접근
--|--|--|--
public | public으로 설정된 멤버(멤버 변수, 멤버 메서드 등)는 자식 클래스에서 접근할 수 있다. | O | O
protected | protected로 설정된 멤버는 자식 클래스에서 접근 가능 | O | X
private | private으로 설정된 멤버는 현재 클래스에서만 접근할 수 있고 자식 클래스에서 접근할 수 없음 | X | X

`public`으로 선언된 요소는 상속이 가능해 자식 클래스에서 접근할 수 있고 객체를 통한 외부 접근이 가능해 개방 정도가 가장 크다.
`protected`로 선언된 멤버는 상속은 가능해 자식 클래스에서 접근할 수 있지만 객체를 통한 외부 접근은 불가능하다.
`private`은 상속이 되지 않아 자식 클래스에서 접근할 수 없고 객체를 통한 외부 접근도 불가능하다.

### 생성자 매개변수에 접근 제한자 추가

생성자에서도 접근제한자를 사용할 수 있다.
생성자로 넘겨지는 인수에 접근 제한자를 사용하게되면 해당 클래스에 그 인수의 식별자 이름과 같은 식별자의 프로퍼티가 정의되고 넘겨진 값으로 할당된다.

```ts
class Cube {
	public width: number
	private length: number
	protected height: number
	constructor(pWidth: number, pLnegth: number, pHeight: number) {
		this.width: pWidth
		this.length = pLength
		this.height = pHeight
	}
}
```
위 코드는 아래 코드와 같다.
```ts
class Cube {
	constructor(public width: number, private lnegth: number, protected height: number) { }
}
```

### 부모 클래스의 멤버를 이용하기

상속 관계가 있을 때 자식 클래스에서 부모 클래스에 선언된 멤버 메서드나 멤버 변수등을 이용 할 수 있는 방법은 `super` 키워드와 `this` 키워드를 이용하는 것이다.
- `super` 키워드: 부모 클래스의 공개 멤버에만 접근할 수 있다.
- `this` 키워드: 부모 클래스에서 상속받은 멤버와 현재 클래스의 멤버 모두에 접근할 수 있다.
```ts
class PC {
	constructor(public hddCapacity: string) { }
	private ram: string = "0G"
	set ramCapacity(value: string) { this.ram = value }
	get ramCapacity() { return this.ram }

	protected getHddCapacity() {
		return this.hddCapacity
	}
}

class Desktop extends PC {
	constructor(public haddCapacity: string) {
		super(hddCapacity)
	}
	getInfo() {
		console.log("1번 HDD 용량: " + super.getHddCapacity(), super.hddCapacity)
		console.log("2번 HDD 용량: " + this.getHddCapacity(), this.hddCapacity)
		this.hddCapacity = "2000G"
		console.log("3번 HDD 용량: " + super.getHddCapacity(), super.hddCapacity)
		console.log("4번 HDD 용량: " + this.getHddCapacity(), this.hddCapacity)
		super.ramCapacity = "16G"
		console.log("5번 RAM 용량: " + this.ramCapacity, super.ramCapacity)
		this.ramapacity = "8G"
		console.log("6번 RAM 용량: " + this.ramCapacity, super.ramCapacity)
	}
}

let myDesktop = new Desktop("1000G")
myDesktop.getInfo()
```
```
// 결과
1번 HDD 용량: 1000G undefined
2번 HDD 용량: 1000G 1000G
3번 HDD 용량: 2000G undefined
4번 HDD 용량: 2000G 2000G
5번 HDD 용량: 16G 16G
6번 HDD 용량: 8G 8G
```
- super 키워드는 부모 클래스의 멤버 변수를 직접 호출할 수 없다. 부모 클래스의 멤버 변수 값을 가져오려면 메서드 / getter을 통해서 가져와야한다.

### public 제한자와 private 제한자

**public**제한자는 클래스 내부와 외부에서 모두 접근할 수 있게 공개하는 접근제한자이다.
부모 클래스로부터 상속도 가능하다.

**private**제한자는 클래스 내부에서는 접근할 수 있지만 외부에서는 접근할 수 없다.

### protected 제한자

**protected** 제한자는 객체를 통한 외부 접근은 허용하지 않지만 상속 관계에서 부모 클래스에 protected로 선언된 메서드나 멤버 변수의 접근을 허용한다.

### 기본 접근 제한자

기본 접근 제한자는 접근 제한자 선언을 생략할 때 사용된다.
기본 접근 제한자가 적용될 수 있는 대상은 클래스의 멤버 변수, 멤버 메서드, 클래스 Get/Set 프로퍼티, 생성자의 매개변수이다.


### 추상클래스를 이용한 공통 기능 정의

추상 클래스(abstract class)는 구현 메서드는 구현 메서드와 추상 메서드(abstract method)가 동시에 존재할 수 있다.
구현 메서드: 실제 구현 내용을 포함한 메서드
추상 메서드: 선언만된 메서드
추상 클래스는 구현 내용이 없는 추상 클래스를 포함하기 떄문에 불완전한 클래스이다. 따라서 추상 클래스는 단독으로 객체를 생성할 수 없다.
**추상 클래스를 상속하고 구현 내용을 추가하는 자식 클래스를 통해 객체를 생성해야한다.**

```ts
abstract class Animal {
	abstract makeSound(): void;
	move(): void {
		console.log("roaming the earth...");
	}
}
```
`abstract`로 표시된 추상 클래스 내의 메소드에는 Implementation이 포함되어있지 않으므로 파생 클래스에서 구현해야한다.
추상 메서드는 `abstract`키워드를 포함해야하며 선택적으로 Getter/Setter를 포함할 수 있다.
추상 클래스에서 추상 멤버 변수가 선언돼 있으면 자식 클래스에서도 선언해야한다.
static이나 private과 함께 선언할 수 없다. => public으로 선언하는 것이 좋다.

추상 클래스에서 선언한 추상 메서드는 오버라이딩해서 자식 클래스에서 반드시 구현해야한다.

```ts
abstract class AbstractBird {

	abstract birdName: string;
	abstract habitat: string;

	abstract flySound(sound: string);

	fly(): void {
		this.flySound("파닥파닥")
	}

	getHabitat(): void {
		console.log(`<${this.birdName}>의 서식지는 <${this.habitat}>입니다.`)
	}

}

class WildGoose extends AbstractBird {
	constructor(public birdName: string, public habitat: string) {
		super()
	}
	flySound(sound: string) {
		console.log(`<${this.birdName}>가 <${sound}> 날아갑니다.`)
	}
}

let wildGoos = new WildGoose("기러기", "순천만 갈대밭");
wildGoose.fly();
wildGoose.getHabitat();
```



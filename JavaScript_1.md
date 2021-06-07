## 자바스크립트의 변수 타입
1. 기본형(Primitive Type) 참조형(Reference Type)으로 나닌다
2. 기본형의 타입으로는 Number, String ,Boolean, null, undefined, Symbol 6가지 타입이있고 
3. 참조형은 Object (Array, Map, Function, Date)등이 있다.

### 데이터 할당
* 변수가 생성될 때, 메모리 공간을 할당하여 그 안에 변수이름에 해당되는 식별자를 입력하고, 그 변수에 저장될 값을 저장하는 과정이 진행된다.
* 이 때 이 변수의 "값"을 식별자가 할당한 메모리(#1)에 직접 입력하지 않고, "값"을 저장하기 위한 메모리(#2)를 따로할당 후 #1(식별메모리) 에 #2(값메모리) 를 입력한다.

### 이렇게 영역을 분리했을때 이점
* 1. 변수 영역에 주소값을 저장하게 되면, 다른 여유 공간의 메모리에 새로운 값을 입력하고 그 주소 값만 가져와 식별자와 연결시키면 되기 때문에 메모리 할당이 가변형인 데이터에 대해 불필요한 추가연산 
필요X
* 2. 자바스크립트에서 변수를 선언하고, **변수 영역**에 식별자를 입력하고, 값을 입력하기 위한 **데이터 영역**에 메모리를 할당할 때, 같은 값이 이미 **데이터 영역**에 저장되어있는지 검색을 한다.
같은 값이 데이터 영역에 이미 저장되어 있다면, 새로 메모리를 할당하지 않고 주소값을 가져다 변수 공간에 입력한다. 이러한 방법은 **중복데이터를 저장하는 경우 효율적**

* undefined = 정의되지 않은 상태 => 변수를 선언했지만 초기화 되지않은 경우 단, 배열의 경우 empty로 취급됨 이 경우 배열을 순회하는 메소드등을 만나면 순회하지 않고 무시
* null = "개발자가 명시적으로 "값이 없음" 그 자체를 의미하기 위한 변수

## 실행 컨텍스트

**실행할 코드들에게 제공할 환경 정보**를 갖는 객체를 실행 컨텍스트라 부른다.

자바스크립트의 어떤 함수가 실행되면, 자바스크립트 엔진은 해당 함수와 관련된 **환경 정보**들을 모아 "실행 컨텍스트"를 구성하고, 이를 Call Stack에 저장한다.

* 자바스크립트 엔진은, Call Stack의 가장 위에 쌓인 데이터를 가지고, 함수를 실행시킨다. 
여기서 엔진이 환경 정보를 제공받아 함수를 실행할 수 있다.

* 자바스크립트 엔진이 수집하는 환경정보는 다음과 같다.
  * VariableEnvironment : 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보 LexicalEnvironment의 스냅샷
  * LexicalEnvironment : 생성된 VariableEnvirontment를 복사하여 만들어, 컨텍스트 내에서 변경 사항이 실시간으로 적용되는 객체  
  * ThisBinding : this 식별자에 할당되는 객체
  
## environmentRecord

해당 컨텍스트에서 **사용될 매개변수, 식별자, 선언한 함수**들이 저장된다.
코드가 실행되기 전 자바스크립트 엔진이 실행컨텍스트를 구성해야 하기때문에 매개변수,식별자, 함수에 대한 정보를 먼저수집해야하는데 이 때 발생하는것이 **호이스팅**이다.

**1. 호이스팅 적용 전 문법**
```jsx
function foo(bar) {
	console.log(bar);
	var bar = 10;
	console.log(bar);
	function bar() {}
	console.log(bar);
}
foo();
```

**2. 호이스팅이 발생시 실제 동작과정(코드가 바뀌진않음)**
```jsx
function foo() {
	var bar; // 매개변수를 이렇게 표현할 수 있음

	console.log(bar);
	var bar = 10;
	console.log(bar);
	function bar() {}
	console.log(bar);
}
foo();
```

## outerEnvironmentReference
현재 실행 컨텍스트의 외부 환경을 저장하는 **outerEnvironmentReference**

### 스코프 
스코프란 식별자에 대한 유효범위로, global과 local로 나뉘고 로컬은 Function과 Block으로 나뉜다.
* function Scope : 정의 된 함수 내에서만 참조 가능
* Block Scope: {} 안에서만 참조가 가능함을 의미한다.

### 스코프 체인
가장 안쪽 스코프에서 바깥 스코프 차례로 이동하며 유효범위를 탐색하는 개념을 **스코프 체인**이라고 부른다.
![image](https://user-images.githubusercontent.com/46732816/120983960-27763b80-c7b5-11eb-9af0-3903bb1f32c7.png)
위와 같이 호출된 함수가 선언될 당신의 외부함수의 스코프를 참조할때 외부함수를 **렉시컬 스코프**라고 하고, 이가 계속적으로 반복되는것을 **스코프 체인**이라고 한다.
### 실행컨텍스트 요약 

* 실행컨텍스트가 실행되는 경우
  * 전역공간에서
  * 함수 호출 시
  * { } 코드 블럭 사용 시 (블록 스코프)
  * eval 함수 사용 시 (잘 쓰이지 않고, 지양하는 코딩 방법)
* 실행 컨텍스트의 생성 단계
  * Compilation Phase
  * Execution Phase

* Creation Phase에서 하는 일
  * LexicalEnvironment 컴포넌트 생성
  * VariableEnvironment 컴포넌트 생성
* Execution Phase에서 하는 일
  * 자바스크립트 엔진이 한줄 한줄 위에서 부터 코드를 읽으면서 코드를 실행
  * 변수들이 값에 할당

## this
this는 기본적으로 실행 컨텍스트가 생성될 때 결정됨
### 함수와 메서드의 차이 
함수는 독립적으로 실행되는 것, 메서드는 특정 객체에 의해서 실행되는 것
같은 코드여도 **함수**로 동작할 수도 있고, **메서드**로 동작할 수도 있음

this는 메서드가 실행될 때에는, 호출 주체가 되는 객체가 this에 할당된다.

``` 
obj1.func(); 
```
이와같은 함수가 실행 될 경우 저 ```.``` 앞의 obj1이 this를 가르친다.

**화살표 함수** 에서 this는 스코프 체인을 통해 가장 가까이 있는 실행 컨텍스트의 this를 참조한다.

**콜백함수** 에서 this는, 기본적으로 전역 객체가 할당되지만 콜백 함수의 주체가 되는 함수에서 내부적으로 tihs를 제어하기 때문에 (call, apply, bind) 개발자가 this값을 추론하기 어려움

**생성자 함수** 생성자 함수에서의 this는, 생성자에 의해서 만들어질 인스턴스 자체를 가르침
```jsx
var Cat = function(name, age) {
	this.bark = '야옹';
	this.name = name;
	this.age = age;
};

var Nabi = new Cat('나비', '2');

console.log(Nabi); // Cat { bark: '야옹', name: '나비', age: '2' }
```

### 명시적으로 this에 값 바인딩하기 
### call, apply, bind

`<function>.call/apply/bind`

셋 다 모두 첫 번째 인자로 this에 바인딩할 값을 입력받음  
call, apply는 this를 바인딩한 후 즉시 실행하며, call은 두 번째 인자부터 this가 바인딩될 함수에 입력할 파라미터를 차례로 넣을 수 있음  
apply는 두 번째 인자에 파라미터를 배열로 입력할 수 있음 bind는 this를 바인딩한 새로운 함수를 반환한다.  
매개 변수는 call과 같은 방식으로 콤마(,)로 구분하여 입력받음  

## 콜백함수
콜백 함수란, 다른 함수나 메서드의 인자로 전달되는 함수 여기서 호출 주체가 되는 **다른함수나 메서드**에 제어권이 넘어가며, 이호출 주체에 의해 콜백 함수의 호출 시점이 결정된다.
 - 콜백함수의 특성 : 콜백함수는, 어떤 객체의 메서드가 전달되더라도 **함수**로서 작동한다.

```jsx
var obj1 = {
	func: function () {
		console.log(this);
	}
};

[1].forEach(obj1.func); // window
```
해당 코드의 실행결과는 obj1이 아닌 **전역객체**가 된다

## 클로저 
* 자신을 내포하는 함수의 컨텍스트에 접근할 수 있는 함수
* 자신이 생성될 때의 스코프에서 알 수 있었던 변수들 중 언젠가 자신이 실행될 때 사용할 변수들만을 기억하며 유지시키는 함수
```jsx
var outer = function () {
	var number = 10;
	var inner = function () {
		console.log(++number);
	}
	inner();
}
outer(); // 11
```
- 해당 코드의 경우 inner()는 outer의 자신의 실행컨텍스트에 있는 스코프체인을 통해 참조할 수 있다.

```
var outer = function () {
	var number = 10;
	var inner = function () {
		return ++number;
	}
	return inner;
}
var closure = outer();
console.log(closure()); // 11
console.log(closure()); // 12
```
내부함수 inner()를 위 와같이 return 하면 이미 생명주기가 끝난 outer 함수안에 있는 지역변수 number의 값에 계속해서 참조할 수 있다.

#### * 클로저의 의미

**자신을 내포하는 함수의 컨텍스트에 접근하는 함수**
**자신이 실행될 때 사용할 변수만을 기억하며 유지시키는 함수**

위에서는 return하는 것으로 outer함수의 LexcialEnvironment를 외부로 전달했는데, 꼭 return만으로 클로저가 발생하지 않는다.
 
브라우저의 전역 객체인 window의 메서드인 `set Interval`이나, `addEventListener`와 같은 함수의 콜백함수로 내부함수를 전달하는 것으로 클로저를 발생시킬 수 있다.

### currying 함수
여러 개의 인자를 받는 함수를, 하나의 인자만 받는 함수 여러 개로 쪼개어 정의하는 방법 
```
var curry2 = func => a => b => func(a, b);
var getMinWith = curry2(Math.min)(10);
console.log(getMinWith(8)); // 8
console.log(getMinWith(25)); // 10
```
a,b 순서대로 인자를 차례대로 입력 받을수 있음, 함수형 프로그래밍에서 **지연실행**이라 부르는 개념

## 프로토타입
자바스크립트의 객체들은 최상위 객체인 Object에 의해서 생성되고, 생성된 하위객체들은 생성자의  
어떤 프로퍼티를 통하여 메소드나 변수를 참조할 수 있는데, 그것이 프로토타입이다.
![image](https://user-images.githubusercontent.com/46732816/120997939-93ab6c00-c7c2-11eb-8586-fcdfe3a5d57f.png)  ![image](https://user-images.githubusercontent.com/46732816/120998133-c0f81a00-c7c2-11eb-81c2-4e173ecc6bc3.png)
`__proto__`는 생략 가능한 프로퍼티이기 때문에, `instance.<property>`의 방식으로 생성자의 `prototype`에 접근 가능하다

```
var instance = new Constructor();
```
* 생성자 함수 `Constructor`를 `new` 연산자와 함께 호출하면
* 생성자 함수에 정의된 내용을 바탕으로, 새로운 인스턴스 `instance`를 생성함
* 이 때 생성된 인스턴스에는 `__proto__`라는 프로퍼티가 부여되는데
* 이 프로퍼티는 생성자 함수의 `prototype`이라는 프로퍼티를 참조한다

`__proto__`의 `constructor`프로퍼티를 통해서, 해당 인스턴스의 생성자 원형을 참조할 수 있는데,  
**number, string, boolean**을 제외하고는 해당 프로퍼티의 값을 바꿀 수 있어서,  
이것으로 원형을 추론하는 것이 항상 안전하지는 않다.

### 메서드 오버라이드
인스턴스는 생성자의 prototype에 정의된 메서드를 같은 이름으로 재정의하여 오버라이드 할 수 있다.
`__proto__`를 통해서 생성자에 있는 원래의 함수에도 접근할 수 있으나, `this`가 달라지기 때문에 호출 방식이 조금 번거로워지긴 함(call, apply 등 사용)

### 프로토타입 체인
prototype 또한 자바스크립트의 최상위 객체인 Object 생성자까지 `__proto__`로 연결되어 있다.  
그래서 자바스크립트의 모든 객체에서, `Object`에 정의되어 있는 `toString()` 함수를 호출할 수 있음

1. <메서드 호출>
2. 현재 프로퍼티에서 호출하려는 메서드를 탐색함
3. 발견되지 않으면 `__proto__`로 이동
4. 이동한 현재 프로퍼티에서 호출하려는 메서드를 탐색함
5. 발견되지 않으면 `__proto__`로 이동
6. ...

```
Object.create()로 생성한 객체는, __proto__가 없는 객체를 생성한다. 다만 Object에 존재하는 내장 메서드는 사용할 수 없게 되지만 객체자체가 경량화됨 
```

인스턴스에서는, 생성자의 prototype이라는 프로퍼티를 참조할 수 있는데. (__proto__ 라는 것으로)  
이것을 통해서 모든 객체가 prototype으로 연결되어 있어 prototype chain이라는 개념이 존재한다.

## 클래스 
* 프로토타입을 통해서 인스턴스가 참조할 수 있는 메서드는 **프로토타입 메서드**
* 생성자에서만 사용되고 인스턴스가 참조할 수 없는 메서드는 **스태틱 메서드**

프로토타입으로 다른 객체지향 언어와 같은 클래스를 구현하고 상속시키는 등이 가능

```jsx
var Rectangle = function (width, height) {
	this.width = width;
	this.height = height;
};

Rectangle.prototype.getArea = function () {
	return this.width * this.height;
}

var rect = new Rectangle(5, 2);
console.log(rect.getArea()); // 10

var Square = function (width) {
	Rectangle.call(this, width, width);
}

Square.prototype = new Rectangle();

var sq = new Square(5);
console.log(sq.getArea()); // 25
```
Sqare 가 Rectanbgle 클래스를 상속하여 만들어짐  
위의 코드의 경우 prototype에 Rectangle로 만들어진 인스턴스가 할당됨  
이 때 Rectangle을 기반으로 생성된 생성자에 Heigh, width가 undefined로 할당된다. (빈값)  

![image](https://user-images.githubusercontent.com/46732816/121000446-306f0900-c7c5-11eb-9a65-69689d8d7329.png)



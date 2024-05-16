## 18-1 일급 객체  

자바스크립트에서 아래와 같은 조건을 만족하는 객체를 **일급 객체** 라고한다.

1. 무명의 리터럴로 생성할 수 있다. 즉 런타임에 생성이 가능하다.
2. 변수나 자료구조에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

자바스크립트의 함수는 위 조건을 전부 만족하므로 일급 객체이다.

```javascript
// 1. 무명의 리터럴로 생성할 수 있다.
// 2. 변수에 저장할 수 있다.
// 런타임에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
  return ++num;
};
const decrease = function (num) {
  return --num;
};
// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };
// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
  let num = 0;
  
  return function () {
    num = aux(num);
    return num;
  };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
console.log(increaser());	// 1
console.log(increaser());	// 2

const decreaser = makeCounter(auxs.decrease);
console.log(decreaser());	// -1
console.log(decreaser());	// -2
```

함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용 할 수 있다는 의미이며 값과 동일하게 취급할 수 있다.

매개변수에 전달하고 반환값으로 사용 할 수 있는 점은 함수형 프로그래밍을 가능하게 하는 자바스크립트의 장점 중 하나이다.

## 18-2 함수 객체의 프로퍼티  

함수는 객체다. `console.dir`로 함수 내 모든 프로퍼티를 확인해보면

![[Pasted image 20240516234639.png]]

좀더 자세하게 `console.log(Object.getOwnPropertyDescriptors(square));` 함수를 사용해서 확인해보면, 

![[Pasted image 20240516235121.png]]

위에서 arguments, caller, length, name, prototype 프로퍼티는 모두 함수 객체의 데이터 프로퍼티이다. `__proto__` 는 `Object.prototype` 객체의 프로퍼티를 상속받은 프로퍼티 이다.
### arguments 프로퍼티  

arguments는 함수 호출시 전달된 인수들의 정보를 담고 있는 이터러블한 유사 배열 객체이다.

함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 `undefined`로 초기화된 이후 인수가 할당된다. 

따라서 매개변수보다 인수의 개수를 전달했을때는 undefined로 매개변수가 존재한다. 많이 전달했을 경우에는 `arguments` 객체의 프로퍼티로 존재한다.

![[Pasted image 20240516235808.png]]

`arguments` 객체는 인자의 개수가 확정적이지 않은 가변 인자 함수를 구현할 대 유용하다.

`arguments`는 유사 배열 객체이기 때문에 배열 메서드를 사용하기 어렵다. 이러한 번거로움을 해결하기 위해 `ES6`에서는 `Rest 파라미터`를 도입했다.

```javascript
function sum2(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum2(1, 2, 3));	// 6
// Rest 파라미터의 도움으로 arguments 객체의 중요성이 떨어졌지만
// 언제나 ES6만 사용하지는 않을 수 있기 때문에 알아두자.
```
### caller 프로퍼티  

비 표준 프로퍼티이며, 호출한 함수 자신을 가리키는 프로퍼티이다.

```javascript
function foo(func) {
  return func();
}
function bar() {
  return 'caller : ' + bar.caller;
}

// 브라우저에서 실행한 결과
console.log(foo(bar));	// caller : function foo(func) {...}
console.log(bar());		// caller : null
```

`caller` 프로퍼티는 함수 자신을 호출한 함수를 가리키므로, `foo` 함수 내에서 호출한 `bar` 함수의 `caller은` `foo` 함수를, 함수 호출 `bar()`의 경우엔 `null`을 가리킨다.
### length 프로퍼티  

선언한 매개변수의 개수를 가리킨다.

```javascript
function foo() {}
console.log(foo.length);	// 0

function baz(x, y){
  return x * y;
}
console.log(baz.length);	// 2
```

`arguments` 의 `length`는 전달받은 인자의 개수여서 갯수가 다를 수 있다.
### name 프로퍼티  

함수의 이름을 나타낸다.

익명 함수 표현식의 경우, `ES5`에서는 빈 문자열, `ES6`에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

```javascript
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name);	// foo

// 익명 함수 표현식
var anonymousFunc = function() {};
// ES5에서 name 프로퍼티는 빈 문자열을 값으로 갖는다.
console.log(anonymousFunc.name);	// anonymousFunc

// 함수 선언문
function bar() {}
console.log(bar.name);	// bar
```
### __proto__ 접근자 프로퍼티  

모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. [[Prototype]] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.

```javascript
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype);	// true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.prototype의 메서드다.
console.log(obj.hasOwnProperty('a'));			// true
console.log(obj.hasOwnProperty('__proto__'));	// false
```
### prototype 프로퍼티

`prototype` 프로퍼티는 `constructor`만이 가지고 있는 프로퍼티다.

```javascript
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype');	// true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype');				// false
```
## 12-1 함수란?  

함수는 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것이다.

수학의 함수처럼 프로그래밍 언어의 함수도 입력을 받아서 출력을 내보낸다. 이때 입력을 전달받는 변수를 **매개변수**, 입력을 **인수**, 출력을 **반환값** 이라고 한다.

함수는 **함수 정의**를 통해 생성한다. 또한 함수의 실행을 명시적으로 지시하는 것을 **함수 호출** 이라고 한다.
## 12-2 함수를 사용하는 이유  

함수는 몇 번이든 호출할 수 있으므로 코드의 재사용이라는 측면에서 유리하다. 이는 유지보수의 편의성, 코드의 신뢰성을 높이는 효과를 준다.

또한 적절한 함수 이름은 코드의 가독성을 향상시킨다.

## 12-3 함수 리터럴  

자바스크립트의 함수는 **객체 타입**의 값이다.

함수 리터럴은 아래의 요소로 구성된다.

- function 키워드
- 함수 이름
- 매개변수 목록
- 함수 몸체

| 구성요소     | 설명                                                                                                                               |
| -------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 함수 이름    | - 함수 이름은 식별자다 .<br>- 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자다.  <br>- 함수 이름은 생략할 수 있다. 이름이 있으면 기명함수 없으면 무명/익명 함수라고 부른다.                   |
| 매개 변수 목록 | - 0개 이상의 매개변수를 소괄호로 감싸고 쉼표로 구분한다.<br>- 각 매개변수에는 함수를 호출할 때 지정한 인수가 순서대로 할당된다.  <br>- 매개변수는 함수 몸체 내에서 변수와 동일, 식별자 네이밍 규칙을 준수 해야한다. |
| 함수 몸체    | - 함수가 호출되었을 때 실행될 문들을 하나의 실행 단위로 정의한 코드 블록  <br>- 함수 호출에 의해 실행된다.                                                                |
일반 객체는 호출할 수 없지만 함수는 호출 할 수 있다.

## 12-4 함수 정의  

함수 정의란 함수를 호출하기 전에 인수를 전달받을 매개변수와 실행할 문들 그리고 반환할 값을 지정하는 것을 말한다.

| 함수 정의 방식        | 예시                                            |
| --------------- | --------------------------------------------- |
| 함수 선언문          | `function add(x,y){   return x+y;   }`        |
| 함수 표현식          | `var add = function(x,y){   return x+y;   };` |
| Function 생성자 함수 | `var add = new Function('x','y','x+y');`      |
| 화살표 함수(ES6)     | `var add = (x,y) => x+y`                      |
변수는 **선언** 하지만 함수는 **정의** 한다고 표현한다.
### 함수 선언문  

함수 선언문은 함수의 이름을 생략 할 수 없다. 또한 함수 선언문은 **표현식이 아닌 문**이다.

```js
function (x, y) {
  return x + y;
};
// SyntaxError
```

함수 선언문은 표현식이 아닌 문이므로 변수에 할당할 수 없다. 하지만 아래 코드를 보면 함수 선언문이 변수에 할당되는 것처럼 보인다.

```js
var add = function add(x, y) {
  return x + y;
};

// 함수 호출
console.log(add(2, 5)); // 7
```

자바스크립트 엔진은 **코드의 문맥**에 따라 동일한 함수 리터럴을 **함수선언문으로 해석**하거나 **함수 리터럴 표현식**으로 해석한다.

`{}` 객체 리터럴은 블록문으로 해석되기도 하고 객체 리터럴로 해석되기도 하는 만큼 **코드의 문맥에 따라 동일한 코드여도 다르게 해석**한다.

```js
// 기명 함수 리터럴을 단독으로 사용하면 함수 선언문으로 해석된다.
// 함수 선언문에서는 함수 이름을 생략할 수 없다.
function foo() { console.log('foo'); }
foo(); // foo

// 함수 리터럴을 피연산자로 사용하면 함수 선언문이 아니라 함수 리터럴 표현식으로 해석된다.
// 함수 리터럴에서는 함수 이름을 생략할 수 있다.
// 아래는 단순 함수 리터럴이고 변수에 할당하지 않았기 때문에 호출 할 수 없다.
(function bar() { console.log('bar'); });
bar(); // ReferenceError: bar is not defined

// 아래와 같이 작성해야 호출 가능하다.
bar = (function bar(){ console.log('bar'); });
bar(); // bar
```

위 예제에서 `foo`는 함수 선언문 `bar`는 함수 표현식으로 해석되는 것 처럼 **기명 함수 리터럴은 코드의 문맥에 따라 다르게 해석된다.**

위에서 `bar`는 중간에 실행하지 못한다 왜냐하면 가리키는 식별자가 없기 때문이다. 그렇다면 `foo`는 어떻게 실행되는 걸까?

함수 선언문으로 정의된 함수를 자바스크립트 엔진은 생성된 함수를 호출하기 위해 **함수이름과 동일한 이름의 식별자를 암묵적으로 생성**하고 거기에 함수 객체를 생성한다.

![[Pasted image 20240511171501.png]]

함수는 함수 이름으로 호출하는 것이 아닌 **함수 객체를 가리키는 식별자로 호출**한다.

![[Pasted image 20240511171716.png]]
### 함수 표현식  

자바스크립트 함수는 [일급객체](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4)이기 때문에 함수를 값처럼 자유롭게 사용 가능하다.

함수 리터럴의 함수 이름은 **생략할 수 있다**. 이러한 함수를 **익명 함수**라 한다.  
함수 표현식의 함수 리터럴은 함수 이름을 생략하는 것이 일반적이다.

```js
// 기명 함수 표현식
var add = function foo (x, y) {
  return x + y;
};

// 함수 객체를 가리키는 식별자로 호출
console.log(add(2, 5)); // 7

// 함수 이름으로 호출하면 ReferenceError가 발생한다.
// 함수 이름은 함수 몸체 내부에서만 유효한 식별자다.
console.log(foo(2, 5)); // ReferenceError: foo is not defined
```

> 함수 표현식은 표현식인 문이며, 함수 선언문은 표현식이 아닌 문이다. 
### 함수 생성 시점과 함수 호이스팅  

```js
// 함수 참조
console.dir(add); // ƒ add(x, y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function (x, y) {
  return x - y;
};
```

위 예제를 보면 알 수 있듯이 함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출할 수 있고, 함수 표현식은 호출 할 수 없다.

함수 선언문과 함수 표현식은 **함수 생성 시점이 다르다**.

함수 선언문은 자바스크립트 엔진에 의해 런타임 전에 미리 함수객체가 생성된다. 이처럼 함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하며 이를 **함수 호이스팅**(`function hoisting`)이라고 한다.

함수 표현식은 변수에 할당되어 사용하므로 변수와 동일하게 **할당문이 실행되는 시점**에 평가되어 함수 객체가 된다.

따라서 함수 표현식으로 함수를 정의하면 **함수 호이스팅**이 아닌 **변수 호이스팅**이 발생한다.
### Function 생성자 함수 

```js
var add = new Function('x', 'y', 'return x + y');

console.log(add(2, 5)); // 7
```

위처럼 함수 생성할 수 있는데 클로저도 안생기고 바람직한 방식이 아니다. 패스
### 화살표 함수  

ES6에서 도입된 화살표 함수는  `function`키워드 대신 화살표 (fat arrow) ` => ` 를 사용해 좀 더 간략한 방법으로 함수를 선언 할 수 있다. 화살표 함수는 항상 익명 함수로 정의한다.

```js
// 화살표 함수
const add = (x, y) => x + y;
console.log(add(2, 5)); // 7
```

화살표 함수는 기존의 함수보다 표현만 간략한 것이 아니라 내부 동작 또한 간략화되어 있다.

1. `this` 바인딩 방식이 다름
2. `prototype` 프로퍼티가 없음
3. `arguments` 객체를 생성하지 않음

화살표 함수에 대해 자세한 것은 26-3에 나와있음.
## 12-5 함수 호출  

### 매개변수와 인수  

함수 외부에서 함수 내부로 필요한 값을 전달할 필요가 있는 경우, **매개변수**를 통해 **인수**를 전달한다. 개수와 타입에 제한이 없다.

```js
function add(x, y) {
  return x + y;
}

// 함수 호출
// 인수 1과 2는 매개변수 x와 y에 순서대로 할당되고 함수 몸체의 문들이 실행된다.
var result = add(1, 2);
```

매개변수는 일반 변수와 마찬가지로 `undefined`로 초기화 된 이후 인수가 순서대로 할당된다.
### 인수 확인  

매개변수와 인수가 서로 개수가 맞는지는 체크하지 않는다. 인수가 부족해서 할당되지 않은 경우에 매개변수의 값은 `undefined` 이다.

자바스크립트는 동적타입 언어 이기 때문에 자바스크립트 함수는 매개변수의 타입을 지정해 둘 수 없다.

단축평가를 활용해 기본값 할당하기

```js 
function add(a, b, c) {
  a = a || 0;
  b = b || 0;
  c = c || 0;
  return a + b + c;
}

console.log(add(1, 2, 3)); // 6
console.log(add(1, 2)); // 3
console.log(add(1)); // 1
console.log(add()); // 0
```

ES6에 도입된 매개변수 기본값을 사용하기

```js
function add(a = 0, b = 0, c = 0) {
  return a + b + c;
}

console.log(add(1, 2, 3)); // 6
console.log(add(1, 2)); // 3
console.log(add(1)); // 1
console.log(add()); // 0
```
### 매개변수의 최대 개수  

매개변수 개수는 명시적으로 제한하고 있지 않지만 일반적으로 개수를 적게 하는게 좋다. 
객체를 인수로 전달하는 것도 개수를 줄이는데 좋은 방법이지만 부수효과가 일어날 수 있기 때문에 주의해야 한다.
### 반환문  

함수 호출은 **표현식**이기 때문에 함수 호출은 값으로 평가될 수 있다.

```js
function multiply(x, y) {
  return x * y; // 반환문
}

// 함수 호출은 반환값으로 평가된다.
var result = multiply(3, 5);
console.log(result); // 15
```

`return` 으로 반환값을 명시적으로 지정하지 않을 시 `undefined`를 반환한다.

## 12-6 참조에 의한 전달과 외부 상태의 변경  

```js
// 매개변수 primitive는 원시값을 전달받고, 매개변수 obj는 객체를 전달받는다.
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = 'Kim';
}

// 외부 상태
var num = 100;
var person = { name: 'Lee' };

console.log(num); // 100
console.log(person); // {name: "Lee"}

// 원시값은 값 자체가 복사되어 전달되고 객체는 참조값이 복사되어 전달된다.
changeVal(num, person);

// 원시값은 원본이 훼손되지 않는다.
console.log(num); // 100

// 객체는 원본이 훼손된다.
console.log(person); // {name: "Kim"}
```

위 예제와 같이 **원시값이 아닌 객체는 매개변수로 전달하고 값을 조작할 시 원본 값이 변경**되는 부수 효과가 있을 수 있으니 주의하자.

객체를 불변객체로 만들어 사용하는 것으로 주로 해결하는데 원본 객체를 완전히 복사하는 깊은 복사를 통해 해결 가능하다.
## 12-7 다양한 함수의 형태  
### 즉시 실행 함수  

함수의 정의와 동시에 즉시 호출되는 함수를 즉시 실행 함수라고 하며 호출 이후 다시 사용할 수 없다.

```js
// 익명 즉시 실행 함수
(function () {
  var a = 3;
  var b = 5;
  return a * b;
}());

// 기명 즉시 실행 함수
(function foo() {
  var a = 3;
  var b = 5;
  return a * b;
}());

foo() // ReferenceError 
```

위처럼 익명함수를 사용하는 것이 일반적이다. 기명함수도 사용할 수 있다.  

그룹연산자`()`내의 기명함수는 함수 선언문이 아니라 함수 리터럴로 평가되며 함수 이름은 함수 몸체에서만 참조가능하여 호출할 수 없다.

```js
// 기명 즉시 실행 함수
(function foo() {
  var a = 3;
  var b = 5;
  return a * b;
}());

foo(); // ReferenceError: foo is not defined
```


즉시 실행 함수는 반드시 그룹 연산자 `()`로 감싸야 한다.

```js
function () { // SyntaxError: Function statements require a function name
  // ...
}();
```

함수 선언문의 형식이 맞지 않는다. 함수 선언문은 함수 이름을 생략할 수 없다.

그렇다면 기명함수를 정의해 호출해 봐도 에러가 난다.

```js
function foo() {
  // ...
}(); // SyntaxError: Unexpected token ')'
```

이유는 세미콜론 자동 삽입 기능 때문인데, 선언문이 끝나는 위치 코드 블럭의 닫는 중괄호 `}`뒤에 `;`세미콜론이 삽입되어 에러가 난다.

```js
function foo() {}(); // => function foo() {};();
```

그룹 연산자`()`만 작성하면 피연산자가 없기 때문에 에러난다.

```js
(); // SyntaxError: Unexpected token )
```

무명이든 기명이든 함수를 그룹 연산자`()`로 감싸면 함수 리터럴로 평가되어 함수 객체가 된다.

```js
console.log(typeof (function f(){})); // function
console.log(typeof (function (){}));  // function
```

즉시 실행 함수도 일반 함수처럼 값을 반환하고 인수도 전달 한다.

```js
// 즉시 실행 함수도 일반 함수처럼 값을 반환할 수 있다.
var res = (function () {
  var a = 3;
  var b = 5;
  return a * b;
}());

console.log(res); // 15

// 즉시 실행 함수에도 일반 함수처럼 인수를 전달할 수 있다.
res = (function (a, b) {
  return a * b;
}(3, 5));

console.log(res); // 15
```

### 재귀 함수  

함수가 자기 자신을 호출하는 것을 재귀 호출 이라고 한다.

탈출 조건이 없다면 함수가 무한정 실행되어 스택 오버플로우가 일어날 것이므로 탈출 조건을 잘 설계하자.

```js
// 함수 표현식
var factorial = function foo(n) {
  // 탈출 조건: n이 1 이하일 때 재귀 호출을 멈춘다.
  if (n <= 1) return 1;
  // 함수를 가리키는 식별자로 자기 자신을 재귀 호출
  return n * factorial(n - 1);

  // 함수 이름으로 자기 자신을 재귀 호출할 수도 있다.
  // console.log(factorial === foo); // true
  // return n * foo(n - 1);
};

console.log(factorial(5)); // 5! = 5 * 4 * 3 * 2 * 1 = 120
```
### 중첩 함수  

함수 내부에 정의된 함수를 중첩함수, 중첨함수를 포함하는 함수를 외부 함수라 부른다.

```js
function outer() {
  var x = 1;

  // 중첩 함수
  function inner() {
    var y = 2;
    // 외부 함수의 변수를 참조할 수 있다.
    console.log(x + y); // 3
  }

  inner();
}

outer();
```

ES6부터는 함수 선언문을 `if`문이나 `for`문 등의 코드 블록 내에서도 정의할 수 있다.  단, 호이스팅으로 인해 혼란이 발생할 수 있으므로 바람직하지 않다.  중첩 함수는 `클로저`와 관련이 있다.
### 콜백 함수  

함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 **콜백 함수**(`callback function`)라고 한다.  

```js
// n만큼 어떤 일을 반복한다
function repeat(n) {
  // i를 출력한다.
  for (var i = 0; i < n; i++) console.log(i);
}

repeat(5); // 0 1 2 3 4
```

위 예제를 아래와 같이 변경하여 공통적으로 수행하는 일을 미리 정의해 재사용할 수 있다.

```js
// 외부에서 전달받은 f를 n만큼 반복 호출한다
function repeat(n, f) {
  for (var i = 0; i < n; i++) {
    f(i); // i를 전달하면서 f를 호출
  }
}

var logAll = function (i) {
  console.log(i);
};

// 반복 호출할 함수를 인수로 전달한다.
repeat(5, logAll); // 0 1 2 3 4

var logOdds = function (i) {
  if (i % 2) console.log(i);
};

// 반복 호출할 함수를 인수로 전달한다.
repeat(5, logOdds); // 1 3
```

위 처럼 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 **고차 함수**(`Higher-Order Function, HOF`)라고 한다.

고차함수는 콜백함수를 자신의 일부분으로 합성하며, 매개변수를 통해 전달받은 콜백함수의 호출시점을 결정해서 호출한다.

> _※고차 함수란?_ 
> -콜백 함수를 전달받은 함수.  
> -콜백 함수를 자신의 일부분으로 합성한다.  
> -전달받은 콜백함수의 호출 시점을 결정해서 호출한다.  
> -콜백 함수에 인수를 전달할 수 있다.


익명함수 리터럴로 정의하면서 곧바로 고차 함수에 전달하는 것이 일반적이다. 하지만 실행때마다 함수가 생성되고 평가되므로, 자주 사용한다면 함수를 따로 정의하고 사용하는 것이 좋다.

```js
// 익명 함수 리터럴을 콜백 함수로 고차 함수에 전달한다.
// 익명 함수 리터럴은 repeat 함수를 호출할 때마다 평가되어 함수 객체를 생성한다.
repeat(5, function (i) {
  if (i % 2) console.log(i);
}); // 1 3
```
### 순수 함수와 비순수 함수

순수 함수는 동일한 인수가 전달되면 언제나 동일한 값을 반환하는 함수다. 또한 외부 상태를 변경하지 않는다.

```js
var count = 0; // 현재 카운트를 나타내는 상태

// 순수 함수 increase는 동일한 인수가 전달되면 언제나 동일한 값을 반환한다.
function increase(n) {
  return ++n;
}

// 순수 함수가 반환한 결과값을 변수에 재할당해서 상태를 변경
count = increase(count);
console.log(count); // 1

count = increase(count);
console.log(count); // 2
```

비순수 함수

```js
var count = 0; // 현재 카운트를 나타내는 상태: increase 함수에 의해 변화한다.

// 비순수 함수
function increase() {
  return ++count; // 외부 상태에 의존하며 외부 상태를 변경한다.
}

// 비순수 함수는 외부 상태(count)를 변경하므로 상태 변화를 추적하기 어려워진다.
increase();
console.log(count); // 1

increase();
console.log(count); // 2
```

외부 상태를 변경하는 부수효과가 있는 비 순수 함수는 코드의 신뢰도를 낮추므로 순수함수를 자주 사용하자.
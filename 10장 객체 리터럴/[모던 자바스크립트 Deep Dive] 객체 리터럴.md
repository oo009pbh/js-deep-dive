## 10-1 객체란?  

자바스크립트를 구성하는 거의 모든 것이 **객체** 이다.

원시 타입의 값은 **변경 불가능 한 값 (immutable value)** 이지만, 객체는 **변경 가능한 값 (mutable value)** 이다.

객체는 0개 이상의 **프로퍼티**로 구성된 집합이다. 프로퍼티는 **키와 값**으로 구성된다.

```javascript
let person = {
  //키   값
  name : 'Lee',  // 프로퍼티
  age  : 20,  // 프로퍼티
};
```

**자바스크립트의 모든 값**은 프로퍼티 값이 될 수 있다. 다시 말해 함수도 프로퍼티 값이 될 수 있다. 프로퍼티 값이 함수일 경우 구분하기 위해 **메서드**라고 부른다.

- 프로퍼티: 객체의 상태를 나타내는 값
- 메서드: 프로퍼티를 참조하고 조작할 수 있는 동작

```javascript
let counter = {
  num: 0,  // 프로퍼티
  increase: function () {   // 메서드
    this.num++;
  }
};
```

이처럼 객체는 프로퍼티와 메서드로 구성된 집합체이다.

## 10-2 객체 리터럴에 의한 객체 생성  

자바스크립트는 **프로토타입 기반 객체지향 언어**로서 다양한 객체 생성 방법을 지원한다.

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

이러한 방법 중에서 가장 일반적이고 간단한 방법은 **객체 리터럴**을 사용하는 방법이다.

객체 리터럴은 중괄호{} 내에 **0개 이상의 프로퍼티**를 정의한다.

```javascript
let person = {
  name: 'Lee',
  sayHello: function () {
    console.log(`Hello! My name is ${this.name}.`);
  }
};

console.log(typeof person) // object
console.log(person) // {name: 'Lee', sayHello: f}
```

이때 객체 리터럴의 중괄호는 **코드 블록을 의미하지 않는다**는 데 주의해야 한다.

객체 리터럴은 값으로 평가되는 표현식 이므로 닫는 중괄호 뒤에 세미콜론을 붙입니다.
## 10-3 프로퍼티  

프로퍼티를 나열할 때는 쉼표(,)로 구분하며, 프로퍼티 키와 프로퍼티 값으로 사용할 수 있는 값은 다음과 같다.

- 프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
- 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값

여기서 중요한 점은 프로퍼티 키는 식별자 네이밍 규칙을 준수하지 않을 때는 **반드시 따옴표를 사용**해야 한다는 점이다.

```js
let person = {
  firstName: 'Hun', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
  last-name: 'Lee'  // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 (SyntaxError)
};
```

프로퍼티 키를 동적으로도 생성 가능하다.

```js
let obj = {};
let key = 'hello';

obj[key] = 'world'; //ES5 프로퍼티 키 동적 생성
// let obj = { [key]: 'world' }; -> ES6 계산된 프로퍼티 이름

console.log(obj); // {hello: "world"}
```

**문자열이나 심벌 외의 값을 키로 사용**시 암묵적 타입 변환을 통해 **문자열이 된다**.

```js
var foo = {
	0: 1,
	1: 2,
	2: 3
}

console.log(foo) // {0: 1, 1: 2, 2: 3}
```

이미 존재하는 프로퍼티 키를 중복선언 할 시 **에러가 발생하지 않고 덮어쓴다**는 점을 주의하자.

```js
var foo = {
	name: 'Lee',
	name: 'Hun',
}

console.log(foo) // {name: "Hun"}
```

## 10-4 메서드  

**프로퍼티 값이 함수**일 경우 일반 함수와 구분하기 위해 메서드라 부르며, 메서드는 객체에 묶여 있는 함수를 의미한다.

```js
let circle = {
  radius: 5,  // 프로퍼티 
  getDiameter: function () {  // 메서드
    return 2 * this.radius;
  }
};
```

## 10-5 프로퍼티 접근  

프로퍼티에 접근하는 방법은 다음과 같이 두 가지이다.

- 마침표 표기법(.)
- 대괄호 표기법([...])

객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다. ReferenceError 가 발생하지 않는 점에 주의가 필요하다.

```js
let person = {
  name: 'lee'
};

console.log(person.age); // undefined
```

식별자 네이밍 규칙을 따르지 않는 프로퍼티 키의 경우 위와 같은 일이 발생 할 수 있다.

```js
let person = {
  'last-name': 'lee',
  1: 10
};

person.'last-name'; // SyntaxError
person.last-name; // 브라우저 환경 NaN
                  // Node.js 환경 ReferenceError: name is not defined
person.[last-name]; // ReferenceError
person.['last-name']; // lee

person.1; // SyntaxError
person.'1' // SyntaxError
person.[1] // 10
person.['1'] // 10
```

`person.last-name;` 의 경우 브라우저 환경에서는 window 전역 객체에 `name` 프로퍼티 가 있기 때문에 참조 에러가 발생하지 않고 **NaN**의 결과가 반환된다.

`node.js` 에서는 스코프 내에 `name` 식별자가 없기 때문에 참조 에러가 발생한다.

## 10-6 프로퍼티 값 갱신  

이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

```js
let person = {
  name: 'lee'
};

person.name = 'kim';

console.log(person); // {name: 'kim'}
```
## 10-7 프로퍼티 동적 생성  

존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당 된다.

```js
let person = {
  name: 'lee'
};

person.age = 20;

console.log(person); // {name: 'lee', age: 20}
```
## 10-8 프로퍼티 삭제  

delete 연산자는 객체의 프로퍼티를 삭제합니다. 만약 존재하지 않는 프로퍼티를 삭제하면 **아무런 에러 없이 무시**된다.

```js
let person = {
  name: 'lee'
};

person.age = 20;

delete person.age;

delete person.address; // 해당하는 프로퍼티가 없어도 에러가 발생하지 않는다.

console.log(person); // {name: 'lee'}
```
## 10-9 ES6에서 추가된 객체 리터럴의 확장 기능  
### 프로퍼티 축약 표현  

프로퍼티 값으로 변수를 사용하는 경우, 변수 이름과 프로퍼티 키가 동일한 이름일때 **프로퍼티 키를 생략**하여 표현할 수 있다.

```js
let x = 1, y = 2;

const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

### 계산된 프로퍼티 이름  

문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있는데 이를 **계산된 프로퍼티 이름**이라고 한다.

```js
// ES5
let prefix = 'prop';
let i = 0;

let obj = {};

obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

ES6에서는 객체 리터럴 내부에서도 프로퍼티 키를 동적 생성 할 수 있다.

```js
// ES6
let prefix = 'prop';
let i = 0;

const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```
### 메서드 축약 표현

ES6 에서는 function 키워드를 생략한 축약 표현을 사용하여 메서드를 만들 수 있다.

```js
const obj = {
  name: 'lee',
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! lee
```
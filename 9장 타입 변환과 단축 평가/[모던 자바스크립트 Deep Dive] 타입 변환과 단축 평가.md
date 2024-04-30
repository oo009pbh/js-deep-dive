## 9-1 타입 변환이란?  

개발자가 의도적으로 값의 타입을 변경하는 것을 **명시적 타입 변환** 또는 **타입 캐스팅**이라 한다.

개발자의 의도와는 상관 없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것은 **암묵적 타입 변환** 혹은 **강제 타입 변환**이라고 한다.
## 9-2 암묵적 타입 변환  

암묵적 타입 변환은 기존 변수 값을 재할당하여 변경하는 것이 아니고 새로운 타입 값을 만들어 한번 사용하고 버린다.
### 문자열 타입으로 변환  

피연산자중 하나 이상이 문자열이라면 아래와 같이 문자열 타입으로 암묵적 타입 변환한다.

```js
// 숫자 타입
0 + ''              // "0"
  -0 + ''             // "0"
1+ ''               // "1"
  -1 + ''             // "-1"
NaN + ''            // "NaN"
Infinity + ''       // "Infinity"
  -Infinity + ''      // "-Infinity"

// boolean 타입
true + ''           // "true"
false + ''          // "false"

// null 타입
null + ''           // "null"

// undefined 타입
undefined + ''      // "undefined"

// 심벌 타입
(Symbol()) + ''     // "TypeError"

// 객체 타입
({}) + ''           // "[object Object]"
Math + ''           // "[object Math]"
[] + ''             // ""
[10, 20] + ''       // "10, 20"
(function(){}) + '' // "function()"
Array + ''          // "function Array() {[native code]}"
```

### 숫자 타입으로 변환  

`-`, `*`, `/`는 모두 산술 연산자인데 해당 연산자를 사용하면 숫자 값을 만든다.

```js
1 - '1' // 0
1 * '10' // 10
1 / 'one' // NaN
```

산술 연산자 사용시 숫자타입이 아닌 피연산자는 암묵적 타입 변환된다.

비교 연산자를 사용해도 마찬가지이다.

```js
'1' > 0 // true
```

`+` 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행한다.

```js
// 숫자 타입
+''               // 0
+'0'              // 0
+'1'              // 1 
+'string'         //  NaN

// boolean 타입
+true             //  1
+false            //  0

// null 타입
+null             //  0

// undefined 타입
+undefined        //  NaN

// 심벌 타입
+Symbol()         //  TypeError

// 객체 타입
+{}               //  NaN
+[]               //  0
+[10, 20]         //  NaN
+(function(){})   //  NaN
```

객체 , 심볼, `undefined` 는 변환되지 않고 NaN이 된다.
null은 0이 된다. (왜..?)
### 불리언 타입으로 변환  

`if`문 `for`문과 같은 제어문 또는 `삼항 조건 연산자`의 조건식은 불리언 값으로 평가되어야 하는 표현식이다.

자바스크립트 엔진은 불리언 타입이 아닌 값을 `Truthy` 값 또는 `Falsy` 값으로 구분해 각각 `true`와 `false`로 암묵적 타입 변환 한다.

- `false`로 평가되는 `Falsy` 값, 아래 값을 제외하고는 전부 Truthy 값이다.
    - `false`
    - `undefined`
    - `null`
    - `0`, `-0`
    - `NaN`
    - `‘’`(빈 문자열)

## 9-3 명시적 타입 변환  

개발자는 의도적으로 아래 방법을 통해 명시적 타입 변환을 할 수 있다.

1) 표준 빌트인 **생성자 함수**를 `new` 연산자 없이 호출하는 방법. `String()`, `Number()`, `Boolean()`  
2) **빌트인 메서드**를 사용하는 방법  
3) **암묵적 타입 변환**을 이용하는 방법
### 문자열 타입으로 변환  

#### `String()` 생성자 함수를 `new` 연산자 없이 호출하는 방법

```js
// 숫자 타입 => 문자열 타입
String(1);              // "1"
String(NaN);            // "NaN"
String(Infinity);       // "Infinity"

// 불리언 타입 ➔ 문자열 타입
String(true)            // "true"
String(false)           // "false"
```

#### `Object.prototype.toString` 메서드를 사용하는 방법

```js
// 숫자 타입 ➔ 문자열 타입
(1).toString();         // "1"
(NaN).toString();       // "NaN"
(Infinity)toString();   // "Infinity"

// 불리언 타입 ➔ 문자열 타입
(true).toString();      // "true"
(false).toString();     // "false"
```

#### 문자열 연결 연산자를 이용하는 방법 (암묵적 타입변환 이용)
 
```js
// 숫자 타입 ➔ 문자열 타입
1 + ''                  // "1"
NaN + ''                //  "NaN"
Infinity + ''           // "Infinity"

// 불리언 타입 ➔ 문자열 타입
true + ''               // "true"
false + ''              // "false"
```
### 숫자 타입으로 변환  

#### `Number()` 생성자 함수를 `new` 연산자 없이 호출하는 방법

```js
// 문자열 타입 ➔ 숫자 타입
Number('0');     // 0
Number('-1');    // -1
Number('10.53'); // 10.53

// 불리언 타입 ➔ 숫자 타입
Number(true);    // 1
Number(false);   // 0
```

#### `parseInt`, `parseFloat`함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)

```js
parseInt('0');       // 0
parseInt('-1');      // -1
parseFloat('10.53'); // 10.53
```

#### `+` 단항 산술 연산자를 이용하는 방법 (암묵적 타입변환 이용)

```js
// 문자열 타입 ➔ 숫자 타입
+'0';     // 0
+'-1';    // -1
+'10.53'; // 10.53

// 불리언 타입 ➔ 숫자 타입
+true;    // 1
+false;   // 0
```

#### `*` 산술 연산자를 이용하는 방법 (암묵적 타입변환 이용)

```js
// 문자열 타입 ➔ 숫자 타입
'0' * 1;     // 0
'-1' * 1;    // -1
'10.53' * 1; // 10.53

// 불리언 타입 ➔ 숫자 타입
true * 1;    // 1
false * 1;   // 0
```

### 불리언 타입으로 변환  

#### `Boolean()` 생성자 함수를 `new` 연산자 없이 호출하는 방법

```js
// 문자열 타입 ➔ 불리언 타입
Boolean('x');       // true
Boolean('');        // false
Boolean('false');   // true

// 숫자 타입 ➔ 불리언 타입
Boolean(0);         // false
Boolean(1);         // true
Boolean(NaN);       // false
Boolean(Infinity);  // true

// null 타입 ➔ 불리언 타입
Boolean(null);      // false

// undefined 타입 ➔ 불리언 타입
Boolean(undefined); // false

// 객체 타입 ➔ 불리언 타입
Boolean({});        // true
Boolean([]);        // true
```

#### `!` 부정 논리 연산자를 두 번 사용하는 방법

```js
// 문자열 타입 ➔ 불리언 타입
!!'x';       // true
!!'';        // false
!!'false';   // true

// 숫자 타입 ➔ 불리언 타입
!!0;         // false
!!1;         // true
!!NaN;       // false
!!Infinity;  // true

// null 타입 ➔ 불리언 타입
!!null;      // false

// undefined 타입 ➔ 불리언 타입
!!undefined; // false

// 객체 타입 ➔ 불리언 타입
!!{};        // true
!![];        // true
```
## 9-4 단축 평가  
### 논리 연산자를 사용한 단축 평가  

논리합`||`와 논리곱`&&` 연산자는 늘 불리언 값으로 평가되지 않는다.

논리합`||`와 논리곱`&&` 연산자는 언제나 **2개의 피연산자중 어느 한쪽으로 평가된다.**

``` js
'cat' && 'dog'   // 'dog'
'cat' || 'dog'   // 'cat'
```

위 코드에서 첫번째 피연산자인 `'cat'` 은 Truthy 값이므로 true로 평가되지만, 이 시점까지는 위 표현식을 평가할 수 없다. 다시 말해 두번째 **피연산자가 논리곱 연산자 표현식의 평가 결과를 결정**한다.
  
이처럼 논리합`||`와 논리곱`&&` 연산자는, 논리 연산의 결과를 결정하는 피연산자를 **타입 변환하지 않고 그대로 반환** 하는데 이를 **단축 평가** 라고 한다.

단축 평가는 표현식을 평가하는 도중에 평가결과가 확정된 경우, **나머지 평가 과정을 생략**한다.

단축평가를 사용하면 `if` 문을 대체할 수 있으며, `삼항 조건 연산자`는 `if-else` 문을 대체할 수 있다.

| 단축 평가 표현식         | 평과 결과    |
| ----------------- | -------- |
| true ¦¦ anything  | true     |
| false ¦¦ anything | anything |
| true && anything  | true     |
| false && anything | false    |
### 옵셔널 체이닝 연산자  

ES11에 도입된 `옵셔널 체이닝` 연산자 (`?.`)는 좌항의 피연산자가 `null` 또는 `undefined`인 경우 `undefined`를 반환하고 그렇지 않으면 우항의 프로퍼티 참조를 이어나간다.

```js
var elem = null;

var value = elem?.value;
console.log(value);        // undefined
```

여기서 기준은 `falsy` 값이 아닌 `null` 또는 `undefined` 이기 때문에 아래와 같은 코드의 작동을 주의하자.

```js
var str = ';

var length = str?.length;
console.log(length);        // 0
```

### null 병합 연산자

ES11에 도입된 `null` 병합 연산자 `??`는 좌항의 피연산자가 `null` 또는 `undefined`인 경우 우항의 피연산자를 반환하고 아니면 좌항의 피연산자를 반환한다.

```js
var foo = null ?? 'default string'
console.log(foo);        // "default string"
```

옵셔널 체이닝과 마찬가지로 기준은 `falsy` 값이 아닌 `null` 또는 `undefined` 이기 때문에 아래와 같은 코드의 작동을 주의하자.

```js
var foo = '' ?? 'default string'
console.log(foo); // ""
```


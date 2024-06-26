## 14.1 변수의 생명 주기  
### 지역 변수의 생명 주기  

변수는 **생명 주기**가 있다. 변수선언은 런타임 이전 단계에서 자바스크립트에서 실행되지만, 엄밀히 말하면 이건 전역변수에 한정되어 맞다.

함수 내 변수들은 함수가 호출된 직후에 함수 몸체의 코드가 실행되기 이전에 자바스크립트 엔진에 의해 먼저 실행된다.

함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료되면 소멸한다.  
즉, **지역 변수의 생명 주기는 함수의 생명 주기와 대부분 일치한다.**

![[Pasted image 20240511190820.png]]

하지만 예외의 경우가 있는데, 클로저의 경우이다.
자바스크립트 가비지 콜렉터는 그 누구도 참조하지 않을 때 메모리공간을 해제하는데. 아래와 같이 스코프를 계속 참조하고 있다면 해제하지 않는다(클로저).

```js
function foo(){
    var x = 10;
    function bar(){
        console.log(x);
    }
    return bar;
}
var bar = foo();
bar(); // 10
```

호이스팅은 스코프 단위로 동작한다.

**호이스팅은 변수 선언이 스코프의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징**이다.
### 전역 변수의 생명 주기  

var 키워드로 선언한 전역변수의 생명 주기는 전역 객체의 생명 주기와 일치한다.

> **전역 객체(global object)**  
> 전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이다. 전역 객체는 **클라이언트 사이드 환경(브라우저)에서는 window, 서버 사이드 환경(Node.js)에서는 global 객체를 의미**한다.

![[Pasted image 20240511191402.png]]
## 14.2 전역 변수의 문제점  

전역변수의 문제점은 아래와 같다.
- 암묵적 결합 : 모든 코드가 전역변수를 참조하고 변경할 수 있는 암묵적 결합을 허용하므로, 코드의 가독성도 나빠지고 의도치 않게 상태가 변경 될 수 있다.
- 긴 생명 주기 : 브라우저가 종료하지 않는다면 메모리를 계속 점유하고 있기 때문에 메모리 리소스를 계속 차지하고 있다.
- 스코프 체인 상에서 종점에 존재 : 변수 검색시간이 상대적으로 오래걸린다.
- 네임스페이스 오염 : 동일한 이름으로 명명된 전역 변수나 전역 함수가 같은 스코프 내에 존재할 경우 예상치 못한 결과를 가져올 수 있다.
## 14.3 전역 변수의 사용을 억제하는 방법  
### 즉시 실행 함수  

모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역변수가 된다.

전역변수를 생성하지 않으므로 라이브러리 등에 자주 사용된다.
### 네임스페이스 객체  

네임스페이스 자체가 전역변수로 할당되므로 좋은 방법은 아니다.

```javascript
var MYAPP = {};  // 전역 네임스페이스 객체

MYAPP.name = 'Lee';

console.log(MYAPP.name);  // Lee
```
### 모듈 패턴  

관련이 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈을 만든다. 클로저를 기반하여 동작하며 캡슐화까지 구현할 수 있다.

캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작 할 수 있는 동작인 메서드를 **하나로 묶는 것**을 의미한다.

객체의 특정 프로퍼티나 메서드를 감출 목적으로도 사용하는데, 이를 **정보 은닉**이라고 한다.

```javascript
var Counter = (function () {
	//private 변수
	var num = 0;
  
  	// 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환한다.
  	return {
    	increase() {
        	return ++num;
        },
      	decrease() {
        	return --num;
        }
    }
}());

//private 변수는 외부로 노출되지 않는다.
console.log(Counter.num); //undefined

console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```
### ES6 모듈

ES6 모듈 사용시 더는 전역변수를 사용 할 수 없다. ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다.

```javascript
<script type='module' scr='lib.mjs'></script>
<script type='module' scr='app.mjs'></script>
```

위와 같이 `script` 태그에 `type=module` 어트립트를 추가하면 로드된 자바스크립트 파일은 모듈로서 동작한다.
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

따라서 매개변수보다 인수의 개수를 전달했을때는 undefined로 매개변수가 존재한다. 많이 전달했을 경우에는 arguments 객체의 프로퍼티로 존재한다.
### caller 프로퍼티  
### length 프로퍼티  
### name 프로퍼티  
### __proto__ 접근자 프로퍼티  
### prototype 프로퍼티
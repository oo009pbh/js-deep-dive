제어문은 조건에 따라 코드 블록을 실행하거나 반복 실행할 때 사용한다.

## 8-1 블록문  

블록문은 **0개 이상의 문을 중괄호로 묶은 것**이다.

```javascript
// 블록문
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```
## 8-2 조건문  

조건문은 주어진 **조건식의 평가**에 의해 블록문의 실행을 결정한다.


### if...else 문  

if 문의 조건식은 불리언 값으로 평가되어야 한다.

만약 if 문의 조건식이 불리언 값이 아닌 값으로 평가되면, 암묵적으로 불리언 값으로 강제 변환되어 실행할 코드 블록을 결정한다.

```javascript
var num = 2;
var kind;

// if 문
if (num > 0) {
  kind = '양수'; // 음수를 구별할 수 없다
}
console.log(kind); // 양수

// if...else 문
if (num > 0) {
  kind = '양수';
} else {
  kind = '음수'; // 0은 음수가 아니다.
}
console.log(kind); // 양수

// if...else if 문
if (num > 0) {
  kind = '양수';
} else if (num < 0) {
  kind = '음수';
} else {
  kind = '영';
}
console.log(kind); // 양수
```

`if ... else` 문은 대부분 삼항 조건 연산자로 대체할 수 있다. 

삼항 조건 연산자는 **값으로 평가될 수 있는 표현식**이고, `if ...else`는 **표현식이 아닌 문**이라는 점이 차이점이다.

### switch 문  

switch 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름을 옮긴다.

```javascript
// 월을 영어로 변환한다. (11 → 'November')
var month = 11;
var monthName;

switch (month) {
  case 1: monthName = 'January'; break;
  case 2: monthName = 'February'; break;
  case 3: monthName = 'March'; break;
  case 4: monthName = 'April'; break;
  case 5: monthName = 'May'; break;
  case 6: monthName = 'June'; break;
  case 7: monthName = 'July'; break;
  case 8: monthName = 'August'; break;
  case 9: monthName = 'September'; break;
  case 10: monthName = 'October'; break;
  case 11: monthName = 'November'; break;
  case 12: monthName = 'December'; break;
  default: monthName = 'Invalid month'; break;
}

console.log(monthName); // November
```

위에서 case문에 해당하는 마지막 문에 break가 없다면 case 문을 실행하고 다음 case문으로 연이어 실행하는 **폴스루**가 발생한다.
## 8-3 반복문  

반복문은 **조건식의 평가 결과가 참**인 경우 코드블록을 실행하고 **그 후 조건식을 다시 평가**하여 여전히 참인 경우 코드블록을 다시 실행한다.
### for 문  

```javascript
for(변수 선언문 또는 할당문; 조건식; 증감식){
    조건식이 참인 경우 반복 실행될 문;
}

for (var i = 0; i < 2; i++) {
  console.log(i);
}
```
### while 문  

`while`문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속 해서 반복 실행한다. `for`문은 반복 횟수가 명확할 때 주로 사용하고 `while`문은 반복횟수가 불명확할 때 주로 사용한다.

```javascript
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
while (count < 3) {
  console.log(count); // 0 1 2
  count++;
}
```
### do...while 문  

`do...while`문은 코드 블록을 먼저 실행하고 조건식을 평가한다. 코드 블록은 무조건 한 번 이상 실행된다.

```javascript
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
do {
  console.log(count);
  count++;
} while (count < 3); // 0 1 2
```
## 8-4 break 문  

`switch`문과 `while`문에서 보았듯이 `break`문을 사용하면 코드 블록을 탈출 한다. 정확히 말하자면 코드 블록을 탈출하는 것이 아니라 레이블 문, 반복문 또는 `switch`문의 코드 블록을 탈출한다.

```javascript
if (true) {
  break; // Uncaught SyntaxError: Illegal break statement
}
```

> [!NOTE]레이블 문(`label statement`)이란
> 식별자가 붙은 문을 말한다.
> 
>  ```javascript
> // foo라는 레이블 식별자가 붙은 레이블 문
> foo: console.log('foo');
> ```
> 
> ```javascript
> // foo라는 식별자가 붙은 레이블 블록문
> foo: {
>   console.log(1);
>   break foo; // foo 레이블 블록문을 탈출한다.
>   console.log(2);
> }
> 
> console.log('Done!');
> ```


## 8-5 continue 문

`continue`문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다. `break`문 처럼 반복문을 탈출하지는 않는다.
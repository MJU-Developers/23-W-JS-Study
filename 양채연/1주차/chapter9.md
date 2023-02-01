# 9. 타입 변환과 단축 평가

### 9.1 타입 변환이란?

-   개발자가 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환** 또는 **타입 캐스팅**이라한다.

```jsx
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅한다.
var str = x.toString();
console.log(typeof str, str); // string 1o

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

-   개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 한다. 이를 **암묵적 타입 변환** 또는 **타입 강제 변환**이라한다.
    -   자바스크립트 엔진은 표현식을 에러 없이 평가하기 위해 피연산자의 값을 암묵적 타입 변환해 새로운 타입의 값을 만들어 단 한 번 사용하고 버린다.

```jsx
var x = 10;

// 암묵적 타입 변환
// 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + "";
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

### 9.2 암묵적 타입 변환

### 9.2.1 문자열 타입으로 변환

```jsx
1 + "2"; // -> "12"
```

### 9.2.2 숫자 타입으로 변환

```jsx
1 - "1"; // -> 0
1 * "10"; // -> 10
1 / "one"; // -> NaN
```

```jsx
"1" > 0; // -> true
```

### 9.2.3 불리언 타입으로 변환

```jsx
if ("") console.log("1");
if (true) console.log("2");
if (0) console.log("3");
if ("str") console.log("4");
if (null) console.log("5");

// 2 4
```

-   **자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)으로 구분한다.**
    -   false로 평가되는 Falsy 값들
    -   false
    -   undefined
    -   null
    -   0, -0
    -   NaN
    -   ‘’ (빈 문자열)

### 9.3 명시적 타입 변환

### 9.3.1 문자열 타입으로 변환

-   문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법은 다음과 같다.
    -   String 생성자 함수를 new 연산자 없이 호출하는 방법
    -   Object.prototype.toString 메서드를 사용하는 방법
    -   문자열 연결 연산자를 이용하는 방법

```jsx
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법

String(1); // -> "1"
String(NaN); // -> "NaN"
String(true); // -> "true"
String(false); // -> "false"

// 2. Object.prototype.toString 메서드를 사용하는 방법
(1)
    .toString()(
        // -> "1"
        NaN
    )
    .toString()(
        // -> "NaN"
        true
    )
    .toString()(
        // -> "true"
        false
    )
    .toString(); // -> "false"

// 3. 문자열 연결 연산자를 이용하는 방법
1 + ""; // -> "1"
NaN + ""; // -> "NaN"
true + ""; // -> "true"
false + ""; // -> "false"
```

### 9.3.2 숫자 타입으로 변환

-   숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법은 다음과 같다.
    -   Number 생성자 함수를 new 연산자 없이 호출하는 방법
    -   parseInt, parseFlaot 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)
    -   -   단항 산술 연산자를 이용하는 방법
    -   -   산술 연산자를 이용하는 방법

```jsx
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
Number("0"); // -> 0
Number("-1"); // -> -1
Number(true); // -> 1
Number(false); // -> 0

// 2. parseInt, parseFlaot 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)
parseInt("0"); // -> 0
parseInt("-1") + // -> -1
    // 3. + 단항 산술 연산자를 이용하는 방법
    "0"; // -> 0
+"-1"; // -> -1
+true; // -> 1
+false; // -> 0

// 4. * 산술 연산자를 이용하는 방법
"0" * 1; // -> 0
"-1" * 1; // -> -1
true * 1; // -> 1
false * 1; // -> 0
```

### 9.3.3 불리언 타입으로 변환

-   불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법은 다음과 같다.
    -   Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
    -   ! 부정 논리 연산자를 두 번 사용하는 방법

```jsx
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
Boolean("x"); // -> true
Boolean("false"); // -> true
Boolean(Infinity); // -> true
Boolean({}); // -> true
Boolean([]); // -> true

// 2. ! 부정 논리 연산자를 두 번 사용하는 방법
!!"x"; // -> true
!!"false"; // -> true
!!Infinity; // -> true
!!{}; // -> true
!![]; // -> true
```

### 9.4 단축 평가

### 9.4.1 논리 연산자를 사용한 단축 평가

-   논리합(||) 또는 논리곱(&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다. 논리합 또는 논리곱 연산자 표현식은 언제나 2개의 피연산자 중 어느 한 쪽으로 평가된다.

```jsx
"Cat" && "Dog"; // -> 'Dog'
```

-   **논리 연산의 결과를 결정하는 두 번째 피연산자, 즉 문자열 ‘Dog’를 그대로 반환한다.**

```jsx
"Cat" || "Dog"; // -> 'Cat'
```

-   **논리 연산의 결과를 결정한 첫 번째 피연산자, 즉 문자열 ‘Cat’을 그대로 반환한다.**

-   **이처럼 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다. 이를 단축 평가라 한다. 단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.**

**[ 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 ]**

```jsx
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null
```

-   객체를 기대하는 변수 값이 null 또는 undefined인 경우 객체의 프로퍼티를 참조하면 타입에러가 발생한다.

```jsx
var elem = null;
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가된다.
```

**[ 함수 매개변수에 기본값을 설정할 때 ]**

-   함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다. 이때 단축 평가를 사용해 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있다.

```jsx
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
    str = str || "";
    return str.length;
}

getStringLength(); // -> 0
getStringLength("hi"); // -> 2

// ES6의 매개변수의 기본값 설정

function getStringLength(str = "") {
    return str.length;
}

getStringLength(); // -> 0
getStringlength("hi"); // -> 2
```

### 9.4.2 옵셔널 체이닝 연산자

-   ES11(ECMAScript2020)에서 도입된 옵셔널 체이닝 연산자 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
    -   이는 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용하다.

```jsx
var elem = null;

// elem이 null 또는 undefined이면 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
var value = elem?.value;
console.log(value); // undefined
```

-   옵셔널 체이닝 연산자는 좌항 피연산자가 false로 평가되는 Fasly 값(false, undefined, null, 0, -0, NaN, ‘’)이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.

```jsx
var str = "";

// 문자열의 길이를 참조한다. 이때 좌항 피연산자가 false로 평가되는 Falsy값이라도
// null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.
var length = str?.length;
console.log(length); // 0
```

### 9.4.3 null 병합 연산자

-   ES11(ECMAScript2020)에서 도입된 null 병합 연산자 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
    -   null 병합 연산자 ??는 변수에 기본값을 설정할 때 유용하다.

```jsx
// 좌항의 피연산자가 null 또는 undefined이면 우항의 피연산자를 반환하고,
// 그렇지 않으면 좌항의 피연산자를 반환한다.
var foo = null ?? "default string";
console.log(foo); // default string
```

-   null 병합 연산자는 좌항의 피연산자가 false로 평가되는 Fasly 값(false, undefined, null, 0, -0, NaN, ‘’)이라도 null 또는 undefined가 아니면 좌항의 피연산자를 그대로 반환한다.

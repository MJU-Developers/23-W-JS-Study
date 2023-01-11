# Chapter.21 빌트인 객체

---

### 자바스크립트 객체의 분류

- **표준 빌트인 객체(standard built-in objects/native objects/global objects)**
  - ECMAScript 사양에 정의된 객체를 말하며, 애플리케이션 전역 공통 기능을 제공한다.
  - 자바스크립트 실행 환경에 관계없이 언제나 사용 가능하다.
  - 전역 객체의 프로퍼티로서 제공된다. (별도 선언 없이 전역 변수처럼 언제나 참조 할 수 있다)
- **호스트 객체**
  - 호스트 객체는 ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경에서 추가로 제공하는 객체를 말한다.
  - 브라우저 환경에서는 DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker와 같은 클라이언트 사이드 Web API를 제공한다.
  - Node.js 환경에서는 Node.js 고유의 API를 제공한다.
- \***\*사용자 정의 객체\*\***
  - 표준 빌트인 객체와 호스트 객체처럼 기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체를 말한다.

## **표준 빌트인 객체**

---

> Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 40여개 표준 빌트인 객체가 있다.

- Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.
- 생성자 함수 객체는 프로토타입 메서드와 정적 메서드를, 아닌 객체는 정적 메서드만 제공한다.
- 생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.
- 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체는 다양한 기능의 빌트인 프로토타입 메서드를 제공한다.
- 표준 빌트인 객체는 인스턴스 없어도 호출 가능한 빌트인 정적 메서드를 제공한다.

# **원시값과 래퍼 객체**

> 원시값은 객체가 아니라 프로퍼티나 메서드를 가질 수 없는데도 원시값인 문자열이 객체처럼 동작하는데 그 이유는 **원시값에 대해 마치 객체처럼 마침표 표기법으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해주기 때문**이다.

**_원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다._**

> **문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체(wrapper object)라 한다.**

- 래퍼 객체의 처리가 종료되면 래퍼 객체의 내부 슬롯에 할당된 원시값으로 원래 상태로 되돌리고, 래퍼 객체는 가비지 컬렉션 대상이 된다.

💡 이처럼 문자열, 숫자, 불리언, 심벌은 암묵적으로 생성되는 래퍼 객체에 의해 마치 객체처럼 사용하고 표준 빌트인 객체의 메서드 또는 프로퍼티를 참조 할 수 있으므로 **String, Number, Boolean 생성자 함수를 new 연산자와 함께 호출하여 인스턴스를 생성하는 것은 권장하지 않는다.**

**_ES6에서 새로 도입된 원시값인 심벌도 래퍼 객체를 생성한다._**

\*`**null`, `undefined`는 래퍼 객체를 생성하지 않는다. (오류가 발생한다.)\*\*\*

# **전역 객체**

> **전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체이다.**

**_브라우저 환경에선 `window`, Node.js 환경에서는 `global`이 전역 객체를 가리킨다._**

### globalThis

> ES11에서 도입된 `**globalThis**`는 여러 환경에서 전역 객체를 가리키던 다양한 식별자를 통일한 식별자다. ECMAScript 표준 사양을 준수하는 모든 환경에서 사용할 수 있다.

```jsx
// 브라우저 환경
globalThis === this; // true
globalThis === window; // true
globalThis === self; // true
globalThis === frames; // true

// Node.js 환경(12.0.0 이상)
globalThis === this; // true
globalThis === global; // true
```

> 전역 객체는 계층적 구조상 어떤 객체에도 속하지 않는 모든 빌트인 객체의 최상위 객체다. 이 말은 전역 객체가 프로토타입 상속 관계상에서 최상위 객체라는 의미가 아니다.

**_전역 객체 자신은 어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 말한다._**

### 전역 객체의 특징

- **전역 객체는 개발자가 의도적으로 생성할 수 없다.**즉 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.
- **전역 객체의 프로퍼티를 참조할 때 `window`(또는 `global`)를 생략할 수 있다.**
- **전역 객체는 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.**
- **자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다.**브라우저 환경에서는 `**DOM**`, **`BOM`**, **`Canvas`**, **`XMLHttpRequest`**, **`fetch`**, **`requestAnimationFrame`**, `**SVG**`, **`Web Storage`**, **`Web Component`**, **`Web Worker`** 같은 클라이언트 사이드 Web API를 호스트 객체로 제공하고 Node.js 환경에서는 Node.js 고유의 API를 호스트 객체로 제공한다.
- **`var` 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.**
  ```jsx
  // var 키워드로 선언한 전역 변수
  var foo = 1;
  console.log(window.foo); // 1
  ```
- **`let`이나 `const` 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.**
- `**let`이나 `const` 키워드로 선언한 전역 변수는 보이지 않는 개념적인 블록 내에 존재하게 된다.\*\*
- **브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 `window`를 공유한다.**여러 개의 script 태그를 통해 코드를 분리해도 하나의 전역 객체 `**window**`를 공유한다.

## 빌트인 전역 프로퍼티(built-in global property)

---

전역 객체의 프로퍼티를 말하며, 주로 애플리케이션 전역에서 사용하는 값을 제공한다.

### Infinity

`**Infinity**` 프로퍼티는 무한대를 나타내는 숫자값 Infinity를 갖는다.

```
console.log(Infinity); // Infinity
// 양의 무한대
console.log(3/0); // Infinity
// 음의 무한대
console.log(-3/0); // -Infinity
// Infinity는 숫자값이다.
console.log(typeof Infinity); // number
```

### NaN

`**NaN**` 프로퍼티는 숫자가 아님(Not-a-Number)을 나타내는 숫자값 NaN을 갖는다. `**NaN**` 프로퍼티는 `**Number.NaN**` 프로퍼티와 같다.

```
console.log(NaN); // NaN
console.log(Number('xyz')); // NaN
console.log(1 * 'string'); // NaN
// NaN는 숫자값이다.
console.log(typeof NaN); // number
```

### undefined

`**undefined**` 프로퍼티는 원시 타입 **`undefined`**를 값으로 갖는다.

```
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined
```

## 빌트인 전역 함수

---

애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서, 전역 객체의 메서드다. 제공하고 있는 빌트인 전역 함수 중에 몇 가지만 간략하게 살펴보자.

### eval

`**eval**` 함수는 자바스크립트 코드를 나타내는 문자열을 인수로 전달받아 런타임에 실행한다.

```
eval('1 + 2;'); // 3
eval('var x = 5;') // undefined

console.log(x); // 5

// 객체 리터럴은 반드시 괄호로 둘러싼다.
const o = eval('({ a: 1 })');
console.log(o); // { a: 1 }

// 함수 리터럴은 반드시 괄호로 둘러싼다.
const f = eval('(function() { return 1; })');
console.log(f()); // 1
```

`**eval**` 함수는 보안에 취약할 뿐만 아니라 또한 `**eval**` 함수를 통해 실행되는 코드는 자바스크립트 엔진에 의해 최적화가 수행되지 않아 일반적인 코드 실행에 비해 처리 속도가 느리기에, **`eval` 함수는 사용하지 않는 것이 좋다.**

### isNaN

전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환한다. 전달받은 인수의 타입이 숫자가 아닌 경우 숫자로 타입을 변환한 후 검사를 수행한다.

```
isNaN(NaN);                   // true
isNaN(10);                    // false
isNaN('10');                  // false  '10' => 10
isNaN('');                    // false  '' => 0
isNaN(true);                  // false  true => 1
isNaN(null);                  // false  null => 0
isNaN(undefined);             // true
isNaN({});                    // true
isNaN(new Date());            // false  new Date() => Number
isNaN(new Date().toString()); // true   String => NaN
```

### encodeURI/decodeURI

URI(Uniform Resource Identifier)는 인터넷에 있는 자원을 나타내는 주소를 말한다. URI의 하위 개념으로 URL, URN이 있다.

![URI구조](https://velog.velcdn.com/images%2Fniyu%2Fpost%2Fc02caaa8-bb99-488a-ae8d-99f3119f215a%2Furi.png)

URI구조

`**encodeURI**` 함수는 URI를 문자열로 전달받아 인코딩을, `**decodeURI**` 함수는 인코딩된 URI를 인수로 전달받아 디코딩한다.

인코딩이란 URI 문자들을 ASCII 문자 셋으로 변환하는 것을 의미한다. 예를 들어 특수 문자인 공백 문자는 `**%20**`, 한글 '가'는 `**%EC%9E%90**`으로 인코딩된다.

```
const uri = 'http://example.com?name=홍길동&job=programmer';

const enc = encodeURI(uri);
console.log(enc); // http://example.com?name=%ED%99%8D%EA%B8%B8%EB%8F%99&job=programmer

const dec = decodeURI(enc);
console.log(dec); // http://example.com?name=홍길동&job=programmer
```

### encodeURIComponent/decodeURIComponent

`**encodeURIComponent**` 함수는 URI 구성 요소를 인수로 전달받아 인코딩한다. 인수로 전달된 문자열을 쿼리 스트링의 일부로 간주하여, 쿼리스트링 구분자로 사용되는 `**=**`, `**?**`, `**&**` 까지 인코딩한다.

반면 `**encodeURI**` 함수는 매개변수로 전달된 문자열을 완전한 URI 전체라고 간주해 쿼리 스트링 구분자는 인코딩하지 않는다.

`**decodeURIComponent**` 함수는 매개변수 전달된 URI 구성 요소를 디코딩한다.

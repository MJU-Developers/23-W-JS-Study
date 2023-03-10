# Chapter.01 프로그래밍

---

> 프로그래밍이란 0과 1밖에 알지 못하는 기계가 실행할 수 있을 정도로 **정확하고 상세하게 요구사항을 설명**하는 작업이며, 그 결과물이 바로 코드다.

# Chapter.02 자바스크립트란?

---

> 자바스크립트는 일반적으로 프로그래밍 언어로써 기본 뼈대를 이루는 ECMAScript와 브라우저가 별도 지원하는 **클라이언트 사이드 Web API** (DOM, BOM, Canvas, XMLHttpRequest, fetch)등을 아우르는 개념이다.

# Chapter.03 자바스크립트 개발환경과 실행 방법

---

> 브라우저와 Node.js의 용도는 다르다. 브라우저는 HTML, CSS, 자바스크립트를 실행해 웹페이지를 브라우저 화면에 렌더링하는 것이 주 목적이지만 Node.js는 브라우저 외부에서 자바스크립트 실행 환경을 제공하는 것이 주된 목적이다.

> 따라서 **브라우저와 Node.js 모두 자바스크립트의 코어인 ECMAScript를 실행할 수 있지만 브라우저와 Node.js에서 ECMAScript 이외에 추가로 제공하는 기능은 호환되지 않는다.**

# Chapter.04 변수

---

## 변수란?

> **변수(Variable)는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름을 말한다. `간단히 말하자면 값의 위치를 가리키는 상징적인 이름이다.`**

## 식별자

> 변수 이름을 **식별자(Identifier)**라고도 한다. **식별자는 어떤 값을 구별해서 식별할 수 있는 고유한 이름을 말한다. `식별자는 값이 아니라 메모리 주소를 기억하고 있다.`**

> 식별자라는 용어는 **변수 이름에만 국한해서 사용하지 않는다.** 예를 들어, 변수, 함수 클래스 등의 이름은 모두 식별자다. **`메모리 상에 존재하는 어떤 값을 식별할 수 있는 이름은 모두 식별자라고 부른다.`**

## 변수 선언

> 자바스크립트 엔진은 변수 선언을 다음과 같은 2단계에 거쳐 수행한다.

- 선언 단계 : 변수 이름을 등록해서 자바스크립트 엔진에 변수의 존재를 알린다.
- 초기화 단계 : 값을 저장하기 위한 메모리 공간을 확보하고 **암묵적으로 undefined를 할당해 초기화한다.**

## 변수 선언의 실행 시점과 변수 호이스팅

> 자바스크립트 엔진은 소스코드를 실행하는 런타임(runtime) 이전에 소스코드 실행을 위한 준비 단계인 소스코드의 평가 과정에서 **변수 선언을 포함한 모든 선언문(변수 선언문, 함수 선언문 등)을 소스코드에서 찾아내 먼저 실행한다.**

> 변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 **변수 호이스팅(variable hoisting)**이라 한다.

## 값의 할당

> 변수 선언은 런타임(runtime) 이전에 실행되지만 **값의 할당은 런타임(runtime)에 실행된다.**

## 값의 재할당

> 재할당은 이미 값이 할당되어 있는 변수에 **새로운 값을 또다시 할당하는 것을 말한다.**

**`값을 재할당할 수 없어서 변수에 저장된 값을 변경할 수 없다면 변수가 아니라 상수(constant)라 한다.`**

> 값을 재할당 한 후 이전에 할당된 **불필요한 값들은** **가비지 콜렉터에 의해 메모리에서 자동해제된다.**

**`단, 메모리에서 언제 해제될지는 예측할 수 없다.`**

## 식별자 네이밍 규칙

- 식별자는 특수문자를 제외한 문자, 숫자, 언더스코어(\_), 달러 기호($)를 포함할 수 있다.
- 단, 식별자는 특수문자를 제외한 문자, 숫자, 언더스코어(\_), 달러 기호($)로 시작해야 한다. 숫자로 시작하는 것은 허용하지 않는다.
- 예약어는 식별자로 사용할 수 없다. `ex) await, break, case, catch, ...etc`

# Chapter.05 표현식과 문

---

## 값

> **값(value)은 표현식(expression)이 평가(evaluate)되어 생성된 결과를 말한다.**

**`평가란 식을 해석해서 값을 생성하거나 참조하는 것을 의미한다.`**

## 리터럴

> **리터럴(literal)은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법(notation)을 말한다.**

## 표현식

> **표현식(expression)은 값으로 평가될 수 있는 문(statement)이다.**

**`표현식이 평가되면 새로운 값을 생성하거나 기존값을 참조한다.`**

## 문

> **문(statement)은 여러 토큰으로 구성되는 프로그램을 구성하는 기본 단위이자 최소 실행 단위다.**

**`토큰(token)이란 문법적인 의미를 가지며, 문법적으로 더 이상 나눌 수 없는 코드의 기본 요소를 의미한다.`**

## 세미콜론과 세미콜론 자동 삽입 기능

> 자바스크립트 엔진이 소스코드를 해석할 때 문의 끝이라고 예측되는 지점에 세미콜론을 자동으로 붙여주는 **세미콜론 자동 삽입 기능 ASI**(automatic semicolon insertion)이 암묵적으로 수행되기 때문에 **문의 끝에 붙이는 세미콜론은 옵션이다.**

# Chapter.06 데이터 타입

> 자바스크립트(ES6)는 7개의 데이터 타입을 제공한다.

## 원시타입

- 숫자(number) 타입
- 문자열(string) 타입
- 불리언(boolean) 타입
- undefined 타입
- null 타입
- 심벌(symbol) 타입

## 객체타입

- 객체
- 함수
- 배열
- ..etc

## 템플릿 리터럴

> ES6부터 **템플릿 리터럴**(template literal)이라고 하는 새로운 문자열 표기법이 도입되었다.

> 템플릿 리터럴은 **백틱**(``)을 사용해 멀티라인 문자열, 표현식 삽입, 태그드 템플릿 등 편리한 문자열 처리 기능을 제공하며 **런타임에 일반 문자열로 변환되어 처리된다.**

```jsx
let first = "Tae-kang";
let last = "Kim";

//ES6: 표현식 삽입
console.log(`My name is ${first} ${last}.`); // My name is Tae-kang Kim.
```

## undefined 타입

> undefined는 개발자가 의도적으로 할당하기 위한 값이 아니라 **자바스크립트 엔진이 변수를 초기화 할 때 사용하는 값이다.**

**`변수를 참조했을 때 undefined가 반환된다면 참조한 변수가 선언 이후 값이 할당된 적이 없는 초기화되지 않은 변수라는 것을 간파할 수 있다.`**

## null 타입

> 프로그래밍 언어에서 **null은 변수에 값이 없다는 것을 의도적으로 명시할 때 사용한다.**

**`변수에 null을 할당하는 것은 이전에 할당되어 있던 값에 대한 참조를 명시적으로 제거하는 것을 의미하며, 자바스크립트 엔진은 참조하지 않는 메모리 공간에 대해 가비지 콜렉션을 수행할 것이다.`**

## 심벌 타입

> 심벌(symbol)은 **ES6에서 추가된 7번째 타입**으로, 변경 불가능한 원시 타입의 값이다.

> 심벌 값은 다른 값과 중복되지 않는 유일무이한 값으로 주로 **이름이 충돌할 위험이 없는 객체의 유일한 프로퍼티 키를 만들기 위해 사용한다.**

## 데이터 타입의 필요성

- 값을 저장할 때 확보해야 하는 **메모리 공간의 크기**를 결정하기 위해
- 값을 참조할 때 한 번에 읽어 들여야 할 **메모리 공간의 크기**를 결정하기 위해
- 메모리에서 읽어 들인 **2진수를 어떻게 해석**할지 결정하기 위해

## 동적 타이핑

> 자바스크립트의 변수는 선언이 아닌 할당에 의해 타입이 결정(타입 추론)된다. 그리고 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있는데 이러한 특징을 동적 타이핑이라 한다.

# Chapter.07 연산자

> 연산자는 하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수, 연산 등을 수행해 하나의 값을 만든다.

**`이 때 연산의 대상을 피연산자라 한다.`**

# Chapter.08 제어문

> 제어문은 조건에 따라 코드 블록을 실행하거나 반복 실행할 때 사용한다.

# Chapter.09 타입 변환과 단축 평가

> 개발자가 **의도적으로 값의 타입을 변환하는 것**을 **명시적 타입 변환**(explicit coercion) 또는 **타입 캐스팅**(type casting)이라 한다.

> 개발자의 **의도와 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환 되는 것**을 **암묵적 타입 변환**(implicit coercion) 또는 **타입 강제 변환**(type coercion)이라 한다.

**`💡 암묵적 타입 변환은 기존 변수 값을 재할당하여 변경하는 것이 아니다.`** 

자바스크립트 엔진은 표현식을 에러없이 평가하기 위해 피연산자의 값을 암묵적 타입 변환해 새로운 타입의 값을 만들어 단 한번 사용하고 버린다.


## 논리 연산자를 사용한 단축 평가

> 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 **단축 평가**(short-circuit evaluation)라 한다.

| 단축 평가 표현식 | 평가 결과 |
| --- | --- |
| true II anything | true |
| false II anything | anything |
| true && anything | anything |
| false && anything | false |

**`💡 객체는 키(key)와 값(value)으로 구성된 프로퍼티(property)의 집합이다.
만약 객체를 가리키기를 기대하는 변수의 값이 객체가 아니라 null 또는 undefinde인 경우 객체의 프로퍼티를 참조하면 타입 에러(TypeError)가 발생한다.`**

**`💡 함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다.
이때 단축 평가를 사용해 매개변수의 기본값을 설정하면 undefinde로 인해 발생할 수 있는 에러를 방지할 수 있다.`**

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
getStringLength("hi"); // -> 2
```

## 옵셔널 체이닝 연산자

> ES11(ECMASciprt2020)에서 도입된 **옵셔널 체이닝**(optional chaining) 연산자 `?.`는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

**`옵셔널 체이닝 연산자 ?.는 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용하다.`**

```jsx
let elem  null;

// elem이 nul 또는 undefined이면 undefined를 반환하고, 그렇지 않으면 우항이 프로퍼티 참조를 이어간다.
let value = elem?.value;
console.log(value); // undefined
```

## null 병합 연산자

> ES11(ECMASciprt2020)에서 도입된 **null 병합**(nullish coalescing) 연산자 `??`는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

**`null 병합 연산자 ??는 변수에 기본값을 설정할 때 유용하다.`**

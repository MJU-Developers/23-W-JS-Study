# 21. 빌트인 객체

## 21.1 자바스크립트 객체의 분류

- 표준 빌트인 객체
- 호스트 객체
- 사용자 정의 객체

## 21.2 표준 빌트인 객체

- Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, Function, Promise, JSON, Error 등 40여 개의 표준 빌트인 객체를 제공함

## 21.3 원시값과 래퍼 객체

- 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌림
- 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라고 함
- null과 undefined는 래퍼 객체를 생성하지 않음

## 21.4 전역 객체

- 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체임
- 브라우저 환경에서는 window(또는 self, this, frames), Node.js 환경에서는 global
- 전역 객체는 개발자가 의도적으로 생성할 수 없음
- 전역 객체의 프로퍼티를 참조할 때 window(또는 global)를 생략할 수 있음
- 전역 객체는 Object, String, Number, Boolean, Function, Array, RegExp, Date, Math, Promise 같은 모든 표준 빌트인 객체를 프로퍼티로 가지고 있음
- 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 가짐
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 됨
- let이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님
- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유함

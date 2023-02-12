# Chapter20 strict mode

### strict mode

- 암묵적 전역implicit global : 암묵적으로 js engine이 전역 객체에 프로퍼티를 동적으로 생성
    - 개발자 의도한 것이 아니므로 오류를 발생시킬 가능성이 높음
- strict mode : js언어의 문법을 좀 더 엄격히 적용해 오류 발생 가능성이 높거나 js 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킴

### strict mode 적용

- 전역의 선두/함수 몸체의 선두에서 use strict 추가
- 전역의 선두 → 전체 스크립트에 적용

### 전역 strict mode 적용 지양

- 전역에 적용한  strict mode 는 스크립트 단위로 적용
- strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있음
    - 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선부에 strict mode적용

### 함수 단위의 strict mode 적용 지양

- 모든 함수에 적용 → 번거로움
- 일부 함수에만 적용 → 바람직하지 않음
- strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는 경우에도 문제 발생
- strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직함

### strict mode가 발생시키는 에러

- 암묵적 전역 → 선언하지 않은 변수 참조시 referenceError발생
- delete연산자로 변수, 함수, 매개변수 삭제시 SyntaxError발생
- 중복된 매개변수 이름 사용시 SyntaxError발생
- with문의 사용 → SyntaxError발생
    - 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름 생략 가능 → 성능과 가독성 저하, 간단한 코드 → 사용을 지양

### strict mode 적용에 의한 변화

- 일반함수로서 호출시 this에 undefined 바인딩
- 매개변수에 전달된 인수를 재할당해 변경하여도 arguments객체에 반영되지 않음
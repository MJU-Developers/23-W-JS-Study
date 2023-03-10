# 13. 스코프

## 13.1 스코프란?

- 모든 식별자는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정
- 스코프는 식별자가 유효한 범위
- 자바스크립트 엔진은 스코프를 통해 이름이 같은 두 개의 변수 중에서 어떤 변수를 참조해야 할 것인지를 결정 = **식별자 결정**
- var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언 가능
- let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언 불가능

## 13.2 스코프의 종류

- 전역 스코프
  - 전역이란 코드의 가장 바깥 영역
  - 전역 변수는 어디서든지 참조 가능
- 지역 스코프
  - 지역이란 함수 몸체 내부
  - 지역 변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효

## 13.3 스코프 체인

- 스코프는 함수의 중첩에 의해 계층적 구조를 가짐
- 스코프 체인: 스코프가 계층적으로 연결된 것
- 변수를 참조할 때, 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 변수를 검색
- 상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없음

## 13.4 함수 레벨 스코프

- 코드 블록이 아닌 함수에 의해서만 지역 스코프가 생성
- var 키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정

## 13.5 렉시컬 스코프

- 자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정
- 함수의 상위 스코프는 언제나 자신이 정의된 스코프
- 함수의 상위 스코프는 함수 정의가 실행될 때 정적으로 결정

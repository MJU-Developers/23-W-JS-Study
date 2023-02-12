# Chapter16 프로퍼티 어트리뷰트

### 내부 슬롯과 내부 메서드

- 내부 슬롯과 내부 메서드 : js engine의 구현 알고리즘을 설명하기 위해 ECMAScript사양에서 사용하는 `의사 프로퍼티와 의사메서드` ([[…]])
- 개발자가 직접 접근할 수 있도록 외부로 공개된 객체의 프로퍼티는 아님
    - 일반적으로 접근 및 호출 방식을 제공하지 않으나 일부에 한해 간접적으로 접근할 수 있는 수단을 제공
- js engine의 내부로직

### 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

- js engine은 프로퍼티 생성 시, 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의
- 프로퍼티의 상태 : 프로퍼티의 value값, 값의 갱신 가능 여부writable, 열거 가능 여부enumerable, 재정의 가능 여부configurable
- 프로퍼티 어트리뷰트 : 내부 상태 값meta-property = 내부 슬롯 [[Value]],[[Writable]],[[Enumerable]],[[Configurable]]
- Object.getOwnPrepertyDescriptor 메서드를 통한 간접적인 확인 가능
    - 매개변수 (객체의 참조, 프로퍼티 키)
    - 프로퍼티의 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환

### 데이터 프로퍼티와 접근자 프로퍼티

- 데이터 프로퍼티 : key, value 로 구성
    - [[Value]],[[Writable]],[[Enumerable]],[[Configurable]]
- 접근자 프로퍼티 : 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자함수accessor function로 구성된 프로퍼티
    - [[Get]],[[Set]],[[Enumerable]],[[Configurable]]
- 프로토타입 : 어떤 객체의 상위 객체의 역할을 하는 객체

### 프로퍼티 정의

- object.definePropert(y/ies) 메소드를 사용해서 정의가능
- 인수(객체의 참조, 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체)

### 객체 변경 방지

- **얉은 변경 방지 :  직속 프로퍼티만 변경 방지, 중첩 객체에는 영향 x**
    - 객체 확장 금지 Object.preventExtenstions : 프로퍼티의 추가 금지
        - 확장 가능한 객체인지 여부 확인 Object.isExtensible
    - 객체 밀봉 Object.seal : 프로퍼티 추가/삭제/프로퍼티 어트리뷰트 재정의 금지, 읽기와 쓰기만 가능
        - 밀봉된 객체인지 여부 확인 Object.isSealed
    - 객체 동결 Object.freeze : 프로퍼티 추가/삭제/프로퍼티 어트리뷰트 재정의/프로퍼티 값 갱신 금지, 읽기만 가능
        - 동결된 객체인지 여부 확인 Object.isFrozen
    
- 불변객체 : 객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용 객체
    - 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 호출
# Chapter.19 프로토타입

---

> 자바스크립트는 명령형(imperative), 함수형(functional), 프로토타입 기반(prototype-based) 객체지향 프로그래밍 (OOP; Object Ori-ented Programming)을 지원하는 멀티 패러다임 프로그래밍 언어다.

자바스크립트는 객체 기반의 프로그래밍 언어이며 **자바스크립트를 이루고 있는 거의 “모든 것”이 객체다.**

## 객체지향 프로그래밍

---

> 객체지향 프로그래밍은 여러 개의 독릭접 단위, 즉 **`객체`**(object)의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말한다.

**_객체지향 프로그래밍은 실세계의 실체(사물이나 개념)를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작한다._**

> 실체는 특징이나 성질을 나타내는 **`속성`**(attribute/property)을 가지고 있고, 이를 통해 실체를 인식하거나 구별할 수 있다.

> 다앙한 속성 중에서 플프로그램에 필요한 속성만 간추려 내어 표현하는 것을 **`추상화`**(abstraction)라 한다.

**_객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조라고 할 수 있다._**

## 상속과 프로토타입

---

> **상속**(inheritance)은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 `**프로퍼티**` 또는 `**메서드**`를 다른 `**객체**`가 상속받아 그대로 사용할 수 있는 것을 말한다.

**_자바스크립트는 `프로토타입(prototype)`을 기반으로 상속을 구현한다._**

```jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592...
console.log(circle2.getArea()); // 12.56637...
```

## 프로토타입 객체

---

> **프로토타입 객체**(또는 줄여서 프로토타입)란 **객체지향 프로그래밍의 근간을 이루는 객체 간 상속(inheritance)을 구현하기 위해 사용된다.**

> 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참조다.

`**모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다.**`

💡 ** 객체는 [[Prototype]] 내부 슬롯에는 직접 접근할 수 없지만, 위 그림 처럼 **proto** 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 자신의 [[Prototype]] 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근할 수 있다.**

- \*\*프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있다.
- 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.\*\*

## **proto** 접근자 프로퍼티

---

> **모든 객체는 **proto** 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.**

- **proto** 는 접근자 프로퍼티다.
- **proto** 접근자 프로퍼티는 상속을 통해 사용된다.
- **proto** 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.
  - 따라서 **proto** 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우에는 Object.getPrototypeOf 메서드를 사용하고, 프로토타입을 교체하고 싶은 경우에는 Object.setPrototypeOf 메서드를 사용할 것을 권장한다.

```cpp
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

## 함수 객체의 prototype 프로퍼티

---

> **함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**

💡 **모든 객체가 가지고 있는(엄밀히 말하면 Object.prototype으로부터 상속받은) **proto** 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.**

> 하지만 이들 프로퍼티를 사용하는 주체가 다르다.

| 구분 | 소유 | 값  | 사용 주체 | 사용 목적 |
| ---- | ---- | --- | --------- | --------- |

| **proto**
접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용 |
| prototype
접근자 프로퍼티 | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

`**객체의 __proto__ 접근자 프로퍼티와 함수 객체의 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.**`

## 프로토타입의 constructor 프로퍼티와 생성자 함수

---

> **모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.**

`**이 연결은 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 때 이뤄진다.**`

`**인스턴스는 프로토타입의 constructor 프로퍼티를 상속받아 사용할 수 있다.**`

## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

---

> **객체 리터럴이 평가될 때는 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하고 프로퍼티를 추가하도록 정의되어 있다.**

💡 Object 생성자 함수 호출과 객체 리터럴의 평가는 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하는 점에서 동일하나 new.target의 확인이나 프로퍼티를 추가하는 처리 등 세부 내용은 다르다.

**따라서 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.**

- **`Function 생성자 함수를 호출하여 생성한 함수는 렉시컬 스코프를 만들지 않고 전역 함수인 것처럼 스코프를 생성하며 클로저도 만들지 않는다.`**
- **`따라서 함수 선언문과 함수 표현식을 평가하여 함수 객체를 생성한 것은 Function 생성자 함수가 아니다.`**

> **하지만 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다. 따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다. 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍(pair)로 존재한다.**

### 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

| 리터럴 표기법      | 생성자 함수 | 프로토타입         |
| ------------------ | ----------- | ------------------ |
| 객체 리터럴        | Object      | Object.prototype   |
| 함수 리터럴        | Function    | Function.prototype |
| 배열 리터럴        | Array       | Array.prototype    |
| 정규 표현식 리터럴 | RegExp      | RegExp.prototype   |

## 프로토타입의 생성 시점

---

> **프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.**

### 사용자 정의 생성자 함수와 프로토타입 생성 시점

---

> 생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

💡 함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행되므로 함수 선언문으로 정의된 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 되고 **이때 프로토타입도 더불어 생성된다.**

`**프로토타입도 객체이고 모든 객체는 프로토타입을 가지므로 프로토타입도 자신의 프로토타입을 갖는다.
생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다.**`

### 빌트인 생성자 함수와 프로토타입 생성 시점

---

> Object, String, Number, Function, Array, RegExp, Date, Promise 등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.

💡 **모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다.**

## 객체 생성 방식과 프로토타입의 결정

---

> 객체는 다음과 같이 다양한 생성 방법이 있다.

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

💡 **각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점이 있다.**

## 프로토타입 체인

---

> **자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색하는데 이를 프로토타입 체인이라 한다.**

_프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 매커니즘이다._

💡 프로토타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototype이다.
따라서 모든 객체는 Object.prototype을 상속받는다.
**Object.prototype을 프로토타입 체인의 종점(end of prototype chain)이라 한다.**

> 자바스크립트 엔진은 객체 간의 상속 관계로 이루어진 프로토타입의 계층적인 구조에서 객체의 프로퍼티를 검색한다.

_따라서 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘이라고 할 수 있다._

> 이에 반해 프로퍼티가 아닌 식별자는 스코프 체인에서 검색한다.
> 자바스크립트 엔진은 함수의 중첩 관계로 이루어진 스코프의 계층적 구조에서 식별자를 검색한다.

_따라서 스코프 체인은 식별자 검색을 위한 메커니즘이라고 할 수 있다._

💡 **이처럼 스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용된다.**

## 오버라이딩과 프로퍼티 섀도잉

---

- 프로토타입 프로퍼티: 프로토타입이 소유한 프로퍼티(메서드 포함)
- 인스턴스 프로퍼티: 인스턴스가 소유한 프로퍼티

> **프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩했고 프로토타입 메서드 sayHello는 가려진다.**

이처럼 상속 관계에 의해 프로퍼티가 가려지는 현상을 프로퍼티 섀도잉(property shadowing)이라 한다.

## **프로토타입의 교체**

---

> 프로토타입은 임의의 다른 객체로 변경 할 수 있다. 자신의 부모 객체를 동적으로 변경할 수 있으며, 객체 간의 상속관계를 변경할 수 있다. 생성자함수, 인스턴스에 의해 교체된다.

### 생성자 함수에 의한 프로토타입 교체

```jsx
const Person = (function () {
  //생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 1 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    sayHello() {
      console.log("hi! my name is ${this.name}");
    },
  };

  return Person;
})();

const me = new Person("Lee");
```

> 위 예시에서는 Person.prototype에 객체 리터럴을 할당하였다. 기존에 생성자함수에 의해 자연스럽게 할당되는 Person.prototype 객체를 임의로 교체한 것이다.

![https://velog.velcdn.com/images%2Frlatp1409%2Fpost%2Fe7f0fa32-756f-4656-9617-e7980ca5b2ec%2FIMG_5782CD35EFE7-1.jpeg](https://velog.velcdn.com/images%2Frlatp1409%2Fpost%2Fe7f0fa32-756f-4656-9617-e7980ca5b2ec%2FIMG_5782CD35EFE7-1.jpeg)

> 프로토타입으로 교체한 객체리터럴에는 constructor 프로퍼티가 없다, 왜냐하면 생성자함수가 있어야하는데 리터럴로 선언되어 기존의 생성자함수 Person을 잃어버렸기 때문에.

_따라서 me 객체의 constructor를 통해 생성자함수를 검색하면 프로토타입 체인에 의해 Person이 아닌 Object가 나온다._

```jsx
console.log(me.constructor === Person); //false
### console.log(me.constructor === Object); //true
```

> 이는 일반적인 constructor 프로퍼티와 생성자 함수간의 연결을 파괴시킨다. 이를 되살리려면 constructor 프로퍼티를 코드내의 추가하여 constructor 프로퍼티를 되살려야한다.

```jsx
const Person = (function () {
  //생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
      console.log("hi! my name is ${this.name}");
    },
  };

  return Person;
})();

const me = new Person("Lee");

console.log(me.constructor === Person); //true
console.log(me.constructor === Object); //false
```

### 인스턴스에 의한 프로토타입의 교체

이는 인스턴스에 `__proto__` (또는 setPrototypeOf)접근자 프로퍼티를 통해 프로토타입을 교체하는 방법이다.

```jsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

const parent = {
  sayHello() {
    console.log("Hi! my name is ${this.name}");
  },
};

// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! my name is Lee
```

> 이 방법 또한 인스턴스의 생성자 함수에 따라 지정된 프로토타입이 아니기 때문에 constructor 프로퍼티가 비어있다.

![https://velog.velcdn.com/images%2Frlatp1409%2Fpost%2F8328e2de-c26a-4b6b-bc03-9b1d31034d70%2FIMG_402A271A02BE-1.jpeg](https://velog.velcdn.com/images%2Frlatp1409%2Fpost%2F8328e2de-c26a-4b6b-bc03-9b1d31034d70%2FIMG_402A271A02BE-1.jpeg)

> 따라서 parent라는 프로토타입이 될 객체 내부에 constructor 프로퍼티와 생성자 함수간의 연결을 설정해주어야 정상적인 프로토타입 체인이 성립된다.

![https://velog.velcdn.com/images%2Frlatp1409%2Fpost%2F19fd989a-1309-4959-8bac-d7f2f8f3da23%2FIMG_3CE6D8EFE435-1.jpeg](https://velog.velcdn.com/images%2Frlatp1409%2Fpost%2F19fd989a-1309-4959-8bac-d7f2f8f3da23%2FIMG_3CE6D8EFE435-1.jpeg)

```jsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

const parent = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
  constructor: Person,
  sayHello() {
    console.log("Hi! my name is ${this.name}");
  },
};

Person.prototype = parent;

//me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); //Hi! my name is Lee
```

> 이처럼 프로토타입 교체를 통해 객체간의 상속관계를 동적으로 변경하는 것은 꽤나 번거롭다.

_따라서 프로토타입을 직접 교체하지 않는 것이 좋다_.

💡 **상속관계를 인위적으로 설정하려면 “직접 상속”을 이용하는 것이 더 편리하고 안전하다.
ES6에서 도입된 클래스를 사용하면 간편하고 직관적으로 상속 관계를 구현할 수 있다.**

## 직접 상속

---

### `Object.create`에 의한 직접 상속

`Object.create` 메서드는

- **명시적으로 프로토타입을 지정**하여 **새로운 객체를 생성**한다.
  - `Object.create` 메서드도 다른 객체 생성 방식과 마찬가지로 추상 연산 `OrdinaryObjectCreate`를 호출한다.
- `Object.create` 메서드의 첫 번째 매개변수에는
  - 생성할 객체의 프로토타입으로 지정할 객체를 전달한다.
- 두 번째 매개변수에는
  - 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달한다.
  - 두 번째 인수는 옵션이므로 생략 가능하다.

```jsx
// 프로토타입이 null인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj → null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지 못한다.
console.log(obj.toString()); // TypeError: obj.toString is not a function

// obj → Object.prototype → null
// obj = {};와 동일하다.
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj → Object.prototype → null
// obj = { x: 1 };와 동일하다.
obj = Object.create(Object.prototype, {
  x: { value: 1, writable: true, enumerable: true, configurable: true },
});
// 위 코드는 다음과 동일하다.
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj → myProto → Object.prototype → null
obj = Object.create(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// obj → Person.prototype → Object.prototype → null
// obj = new Person('Lee')와 동일하다.
obj = Object.create(Person.prototype);
obj.name = "Lee";
console.log(obj.name); // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
```

`Object.create` 메서드는 첫 번째 매개변수에 전달된 객체의 프로토타입 체인에 속하는 객체를 생성한다. 즉, **객체를 생성하면서 직접적으로 상속을 구현**하는 것이다. 이 메서드의 장점은 다음과 같다.

- `new` 연산자가 없이도 객체를 생성할 수 있다.
- 프로토타입을 지정하면서 객체를 생성할 수 있다.
- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

### 객체 리터럴 내부에서 `proto`에 의한 직접 상속

`Object.create` 메서드에 의한 직접 상속은 여러 장점이 있지만,두 번째 인자로 프로퍼티를 정의하는 것은 번거롭기 때문에**ES6에서는 객체 리터럴 내부에서 `__proto__` 접근자 프로퍼티를 사용하여 직접 상속을 구현**할 수 있다.

```jsx
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto,
};

/** 위 코드는 아래와 동일하다.
const obj = Object.create(myProto, {
  y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

## 정적 프로퍼티/메서드

---

정적(static) 프로퍼티/메서드는 **생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드**를 말한다.

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = "static prop";

// 정적 메서드
Person.staticMethod = function () {
  console.log("staticMethod");
};

const me = new Person("april");

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

MDN과 같은 문서를 보면 **정적 프로퍼티/메서드와 프로토타입 프로퍼티/메서드를 구분**해놓고 있으므로 표기법만으로도 정적 프로퍼티/메서드와 프로토타입 프로퍼티/메서드를 구별할 수 있어야 한다.

![https://velog.velcdn.com/images/april_5/post/3e51d6c7-9c10-4770-b029-8d9953f20e53/image.png](https://velog.velcdn.com/images/april_5/post/3e51d6c7-9c10-4770-b029-8d9953f20e53/image.png)

참고로 프로토타입 프로퍼티/메서드를 표기할 때 `prototype`을 `#`으로 표기하는 경우도 있으니 참고하자

- 예시) `Object.prototype.isPrototypeof`를 `Object#isPrototypeof` 으로 표기

## 프로퍼티 존재 확인

---

### `in` 연산자

`in` 연산자는 객체 내에 **특정 프로퍼티가 존재하는지 여부**를 확인한다.

```
/**
* key: 프로퍼티 키를 나타내는 문자열
* object: 객체로 평가되는 표현식
*/
key in object
```

```
const person = {
  name: 'Lee',
  address: 'Seoul'
};

// person 객체에 name 프로퍼티가 존재한다.
console.log('name' in person);    // true
// person 객체에 address 프로퍼티가 존재한다.
console.log('address' in person); // true
// person 객체에 age 프로퍼티가 존재하지 않는다.
console.log('age' in person);     // false
```

`in` 연산자는 **프로토타입 체인 상에 존재하는 모든 프로토타입에서 프로퍼티를 검색**한다.

### `Object.prototype.hasOwnProperty` 메서드

`Object.prototype.hasOwnProperty` 메서드를 사용해도 객체에 **특정 프로퍼티가 존재하는지 확인**할 수 있다.

- 인수로 전달받은 프로퍼티 키가 **객체 고유의 프로퍼티 키인 경우에만** `true`를 반환하고
- 상속받은 프로토타입의 프로퍼티인 경우 `false`를 반환한다.

## 프로퍼티 열거

---

### `for .. in` 문

객체의 **모든 프로퍼티를 순회하며 열거**하려면 `for .. in` 문을 사용한다

```
for (변수선언문 in 객체) { ... }
```

`for .. in` 문은

- 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서
- 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 `true`인 프로퍼티를 순회하며 열거한다.

```
const person = {
  name: 'Lee',
  address: 'Seoul',
  __proto__: { age: 20 }
};

for (const key in person) {
  // 객체 자신의 프로퍼티인지 확인한다.
  if (!person.hasOwnProperty(key)) continue;
  console.log(key + ': ' + person[key]);
}
// name: Lee
// address: Seoul
```

### `Object.keys`/`values`/`entries` 메서드

`Object.keys`와 `Object.values` 메서드는

- 객체 자신의 열거 가능한 **프로퍼티 키와 값**을 **배열로 반환**한다.
- ES8에서 도입된 `Object.entries` 메서드는
  - 객체 자신의 열거 가능한 **키와 값의 쌍의 배열을 반환**한다

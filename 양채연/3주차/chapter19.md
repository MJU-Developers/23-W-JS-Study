# 19. 프로토타입

-   자바스크립트는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어다.

    -   클래스 기반 객체지향 프로그래밍 언어보다 효율적이며 더 강력한 객체지향 프로그래밍 능력을 지니고 있는 프로토타입 기반의 객체지향 프로그래밍 언어다.

-   자바스크립트는 객체 기반의 프로그래밍 언어이며 **자바스크립트를 이루고 있는 거의 “모든 것”이 객체다.**
    -   원시 타입의 값을 제외한 나머지 값들(함수, 배열, 정규표현시기 등)은 모두 객체다.

### 19.1 객체지향 프로그래밍

-   객체지향 프로그래밍은 명령형 프로그래밍의 절차지향적 관점에서 벗어나 여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말한다.

-   객체지향 프로그래밍은 실세계의 실체를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작된다. 실체는 특징이나 성질을 나타내는 **속성**을 가지고 있고, 이를 통해 실체를 인식하거나 구별할 수 있다.

-   다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 **추상화**라 한다.

```jsx
// 이름과 주소 속성을 갖는 객체
const person = {
    name: "Lee",
    address: "Seoul",
};

console.log(person); // {name: "Lee", address: "Seoul"}
```

-   이처럼 **속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조**를 객체라 하며, 객체 지향 프로그래밍은 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다.

```jsx
const circle = {
    radius: 5, // 반지름

    // 원의 지름: 2r
    getDiameter() {
        return 2 * this.radius;
    },
};

console.log(circle); // {radius: 5, getDiameter: f}
```

-   이처럼 객체지향 프로그래밍은 객체의 **상태**를 나타내는 데이터와 상태 데이터를 조작할 수 있는 **동작**을 하나의 논리적인 단위로 묶어 생각한다.

-   객체는 **상태 데이터와 동작을 하나의 논리로 묶은 복합적인 자료구조**라고 할 수 있다.
    -   객체의 상태 데이터를 프로퍼티, 동작을 메서드라 부른다.

### 19.2 상속과 프로토타입

-   상속은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로터피 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.
-   자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다.

-   동일한 생성자 함수에 의해 생성된 모든 인스턴스가 동일한 메서드를 중복 소유하는 것은 메모리를 불필요하게 낭비한다. 또한 인스턴스를 생성할 때마다 메서드를 생성하므로 퍼포먼스에도 악영향을 준다.

![스크린샷 2023-01-31 오후 4.22.57.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4711421b-94d4-472c-b31b-a8ca66e7cff8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-31_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.22.57.png)

-   상속을 통해 불필요한 중복을 제거해 보자. **자바스크립트는 프로토타입을 기반으로 상속을 구현한다.**

![스크린샷 2023-01-31 오후 4.23.06.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b7877781-edbe-49f5-ae8e-746530de7f6e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-31_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.23.06.png)

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

console.log(circle1.getArea()); // 3.141592..
console.log(circle2.getArea()); // 12.56637..
```

-   Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위(부모) 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받는다.

-   상속을ㄴ 코드의 재사용이란 관점에서 매우 유용하다. 생성자 함수가 생성할 모든 인스턴스가 공통적으로 사용할 프로퍼티나 메서드를 프로토타입에 미리 구현해 두면 생성자 함수가 생성할 모든 인스턴스는 별도의 구현 없이 상위(부모) 객체인 프로토타입의 자산을 공유하여 사용할 수 있다.

### 19.3 프로토타입 객체

-   프로토타입 객체(=프로토타입)란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다.
-   모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참조다. [[Prototype]]에 저장되는 프로토타입은 객체 생성 방식에 의해 결정된다.

    -   즉, 객체가 생성될 대 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장된다.

-   모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다.

    -   [[Prototype]] 내부 슬롯에는 직접 접근할 수 없지만 ** proto ** 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 자신의 [[Prototype]] 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근할 수 있다.

    -   그리고 프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고, 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.

![스크린샷 2023-01-31 오후 4.43.42.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5449e1d1-b68e-4d93-bbd6-d8451ee5dba2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-31_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.43.42.png)

### 19.3.1 ** proto ** 접근자 프로퍼티

-   **모든 객체는 ** proto ** 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.**
    -   다음 예제를 크롬 브라우저의 콘솔에서 출력하면 다음과 같다.

```jsx
const person = { name: "Lee" };
```

![스크린샷 2023-01-31 오후 4.47.11.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a6917110-8166-4956-86a9-742ed111a64f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-31_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.47.11.png)

-   이는 ** proto ** 접근자 프로퍼티를 통해 person 객체의 [[Prototype]] 내부 슬롯이 가리키는 객체인 Object.prototype에 접근한 결과를 크롬 브라우저가 콘솔에 표시한 것이다.
-   이처럼 모든 객체는 ** proto ** 접근자 프로퍼티를 통해 프로토타입을 가리키는 [[Prototype]] 내부 슬롯에 접근할 수 있다.

\***\* proto **는 접근자 프로퍼티다.\*\*

-   자바스크립트는 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공한다.
-   [[Prototype]] 내부 슬롯에도 직접 접근할 수 없으며 ** proto ** 접근자 프로퍼티를 통해 간접적으로 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근할 수 있다.

-   Object.prototype의 접근자 프로퍼티인 ** proto **는 getter/setter 함수라고 부르는 접근자 함수 ([[Get]], [[Set]] 프로퍼티 어트리뷰트에 할당된 함수)를 통해 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입을 취득하거나 할당한다.

```jsx
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;

// setter 함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

\***\* proto ** 접근자 프로퍼티는 상속을 통해 사용된다.\*\*

-   ** proto ** 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다.
-   모든 객체는 상속을 통해 Object.prototype.**proto** 접근자 프로퍼티를 사용할 수 있다.

```jsx
const person = { name: "Lee" };

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다.
console.log(person.hasOwnProperty("__proto__")); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));
// {get: f, set: f, enumerate: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```

\***\* proto ** 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유\*\*

-   [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다.

```jsx
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

-   이러한 코드가 에러 없이 정상적으로 처리되면 서로가 자신의 프로토타입이 되는 비정삭적인 프로토타입 체인이 만들어지기 때문에 ** proto ** 접근자 프로퍼티는 에러를 발생시킨다.

    -   순환 참조하는 프로토타입 체인이 만들어지면 프로토타입 체인 종점이 존재하지 않기 때문에 프로토타입 체인에서 프로퍼티를 검색할 때 무한 루프에 빠진다.

-   프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다.

\***\* proto ** 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.\*\*

-   모든 객체가 ** proto ** 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문이다.
-   직접 상속을 통해 Object.prototype을 상속받지 않는 객체를 생성할 수도 있기 때문에 ** proto ** 접근자 프로퍼티를 사용할 수 없는 경우가 있다.

```jsx
// obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__ 를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__); // undefined

// 따라서 __proto__ 보다 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```

-   따라서 ** proto ** 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우에는 Object.getPrototypeOf 메서드를 사용하고, 프로토타입을 교체하고 싶은 경우에는 Object.setPrototypeOf 메서드를 사용할 것을 권장한다.

```jsx
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

### 19.3.2 함수 객체의 prototype 프로퍼티

-   **함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 기리킨다.**

    -   non-constructor인 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.

-   **모든 객체가 가지고 있는(엄밀히 말하면 Object.prototype으로부터 상속받은) ** proto ** 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 기리킨다.**

```jsx
// 생성자 함수
function Person(name) {
    this.name = name;
}

const me = new Person("Lee");

// 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__); // true
```

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

-   모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 기리킨다.
-   이 연결은 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 대 이뤄진다.

```jsx
// 생성자 함수
function Person(name) {
    this.name = name;
}

const me = new Person("Lee");

// me 객체의 생성자 함수는 Person 이다.
console.log(me.contructor === Person); // true
```

-   me 객체는 프로토타입인 Person.prototype의 constructor 프로퍼티를 상속받아 사용할 수 있다.

### 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

-   리터럴 표기법에 의해 생성된 객체도 물론 프로토타입이 존재한다. 하지만 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수는 없다.

```jsx
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true
```

-   Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전달하면서 호출하면 내부적으로는 추상 연산 OrdinartObjectCreate를 호출하여 Object.prototype을 프로토타입으로 갖는 빈 객체를 생성한다.

```jsx
// 2. Object 생성자 함수에 의한 객체 생성
// 인수가 전달되지 않았을 때 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성한다.
let obj = new Obejct();
console.log(obj); // {}

// 3. new.target이 undefined나 Object가 아닌 경우
// 인스턴스 -> Foo.prototype -> Objec.prototype 순으로 프로토타입 체인이 형성된다.
class Foo extends Object {}
new Foo(); // Foo {}

// 3. 인수가 전달된 경우에는 인수를 객체로 변환한다.
// Number 객체 생성
obj = new Object(123);
console.log(obj); // Number(123)

// String 객체 생성
obj = new Object("123");
console.log(obj); // String("123")
```

-   객체 리터럴이 평가될 때는 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하고 프로퍼티를 추가하도록 정의되어 있다.

-   이처럼 Object 생성자 함수 호출과 객체 리터럴의 평가는 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하는 점에서 동일하나 new.target의 확인이나 프로퍼티를 추가하는 처리 등 세부 내용은 다르다.

-   객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.

-   마찬가지로 함수 선언문과 함수 표현식을 평가하여 함수 객체를 생성한 것은 function 생성자 함수가 아니다. 하지만 constructor 프로퍼티를 통해 확인해보면 foo 함수의 생성자 함수는 Function 생성자 함수다.

```jsx
// foo 함수는 Function 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 생성했다.
function foo() {}

// 하지만 constructor 프로퍼티를 통해 확인해보면 함수 foo의 생성자 함수는 Function 생성자 함수다.
console.log(foo.constructor === Function); // true
```

-   리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다. 따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다.
-   프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 때문이다.

-   다시 말해, **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.**

-   큰 틀에서 보면 리터럴 표기법으로 생성한 객체도 생성자 함수로 생성한 객체와 본질적인 면에서 큰 차이는 없다.

### 19.5 프로토타입의 생성 시점

-   리터럴 표기법에 의해 생성된 객체도 생성자 함수와 연결되는 것을 살펴보았다.
-   객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결되어 있다.

-   **프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.**
-   생성자 함수는 사용자가 직접 정의한 사용자 정의 생성자 함수와 자바스크립트가 기본 제공하는 빌트인 생성자 함수로 구분할 수 있다.

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

-   내부 메서드 [[Construct]]를 갖는 함수 객체, 즉 화살표 함수나 ES6의 메서드 축약 표현으로 정의하지 않고 일반 함수(함수 선언문, 함수 표현식)로 정의한 함수 객체는 new 연산자와 함께 생성자 함수로서 호출할 수 있다.

-   **생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다. 생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다.**
    -   생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor는 프로토타입이 생성되지 않는다.

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

-   Object, String, Number, Function, Array, RegExp, Date, Promise 등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.
    -   모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다.

### 19.6 객체 생성 방식과 프로토타입의 결정

-   객체는 다음과 같이 다양한 생성 방법이 있다.

    -   객체 리터럴
    -   Object 생성자 함수
    -   생성자 함수
    -   Object.create 메서드
    -   클래스(ES6)

-   다양한 방식으로 생성된 모든 객체는 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점이 있다.

-   프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정된다.

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

-   자바스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할 때 추상 연산 OrdinaryObjectCreate를 호출한다. 이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.

    -   즉, 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.

-   객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 되며. 이로써 Object.prototype을 상속받는다.

-   obj 객체는 constructor 프로퍼티와 hasOwnProperty 메서드를 소유하지 않지만 자신의 프로토타입인 Object.prototype의 construct 프로퍼티와 hasOwnProperty 메서드를 자신의 자산인 것 처럼 자유롭게 사용할 수 있다.

-   이는 obj 객체가 자신의 프로토타입인 Object.prototype 객체를 상속받았기 때문이다.

```jsx
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

-   Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상 연산 OrdinaryObjectCreate가 호출된다. 이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.

    -   즉, Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.

-   Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 되며, 이로써 Object.prototype을 상속받는다.

```jsx
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

-   객체 리터럴과 Object 생성자 함수에 의한 객체 생성 방식의 차이는 프로퍼티를 추가하는 방식에 있다.
    -   객체 리터럴 방식은 객체 리터럴 내부에 프로퍼티를 추가하지만
    -   Object 생성자 함수 방식은 일단 빈 객체를 생성한 이후 프로퍼티를 추가해야 한다.

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

-   new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 다른 객체 생성 방식과 마찬가지로 추상 연산 OrdinaryObjectCreate가 호출된다. 이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.
    -   즉, 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

![스크린샷 2023-02-01 오후 5.12.02.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/233b7d93-9c37-4059-a1a8-a7be6b48a4cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-02-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.12.02.png)

-   표준 빌트인 객체인 Object 생성자 함수와 더불어 생성된 프로토타입 Object.prototype은 다양한 빌트인 메서드(hasOwnProperty, ..)를 갖고 있다. 하지만 사용자 정의 생성자 함수 Person 과 더불어 생성된 Person.prototype의 프로퍼티는 constructor 뿐이다.

-   프로토타입 Person.prototype에 프로퍼티를 추가하여 하위(자식)객체가 상속받을 수 있도록 구현해보자

-   프로토타입은 객체다. 따라서 일반 객체와 같이 프로토타입에도 프로퍼티를 추가/삭제할 수 있다. 그리고 업데이트되니 프로퍼티는 프로토타입 체인에 즉각 반영된다.

```jsx
function Person(name) {
    this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
const you = new Person("Kim");

me.sayHello(); // Hi! My name is Lsee
you.sayhello(); // Hi! My name is Kim
```

-   Person 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 sayHello 메서드를 상속받아 자신의 메서드처럼 사용할 수 있다.

![스크린샷 2023-02-01 오후 5.19.22.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1825ed4c-5199-499b-a7e6-65252b9c27f8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-02-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.19.22.png)

### 19.7 프로토타입 체인

```jsx
function Person(name) {
    this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty("name")); // true
```

-   Person 생성자 함수에 의해 생성된 me 객체는 Object.prototype의 메서드인 hasOwnProperty를 호출할 수 있다.
-   이것은 me 객체가 Person.prototype뿐만 아니라 Object.prototype도 상속받았다는 것을 의미한다.
    -   me 객체의 프로토타입은 Person.prototype이다.
    -   Person.prototype의 프로토타입은 Object.prototype이다.
    -   프로토타입의 프로토타입은 언제나 Object.prototype이다.

![스크린샷 2023-02-01 오후 5.23.19.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ea5b8966-17bb-4751-a328-abee12d7190b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-02-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.23.19.png)

-   **자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 이를 프로토타입 체인이라 한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.**

```jsx
// hasOwnProperty는 Object.prototype의 메서드다
// me 객체는 프로토타입 체인을 따라 hasOwnPrototype 메서드를 검색하여 사용한다.
me.hasOwnProperty("name"); // -> true
```

me.hasOwnProperty(’name’)과 같이 메서드를 호출하면 자바스크립트 엔진은 다음과 같은 과정을 거쳐 메서드를 검색한다. (프로퍼티를 참조하는 경우도 마찬가지)

1. 먼저 hasOwnProperty를 호출한 me 객체에서 hasOwnProperty 메서드를 검색한다. me 객체에는 hasOwnProperty 메서드가 없으므로 프로토타입 체인을 따라, 다시 말해 [[Prototype]] 내부 슬롯에 바인딩되어 있는 프로토타입(위 예제의 경우 Person.prototype)으로 이동하여 hasOwnProperty 메서드를 검색한다.

1. Person.prototype에도 hasOwnProperty 메서드가 없으므로 프로토타입 체인을 따라, 다시 말해 [[Prototype]] 내부 슬롯에 바인딩되어 잇는 프로토타입(위 예제의 경우 Object.prototype)으로 이동하여 hasOwnProperty메서드를 검색한다.

1. Object.prototype 에는 hasOwnProperty 메서드가 존재한다. 자바스크립트 엔진은 Object.prototype.hasOwnProperty 메서드를 호출한다. 이때 Object.prototype.hasOwnProperty메서드의 this에는 me 객체가 바인딩된다.

-   프로토타입 체인의 최상위에 위치나는 객체는 언제나 Object.prototype이다. 따라서 모든 객체는 Object.prototype를 상속받는다.

-   Object.prototype을 프로토타입 체인의 종점이라 한다.

    -   Object.prototype의 프로토타입, 즉 [[Prototype]] 내부 슬롯의 값은 null이다.

-   Object.prototype에서도 프로퍼티를 검색할 수 없는 경우 undefined를 반환한다.

-   자바스크립트 엔진은 객체 간의 상속 관계로 이루어진 프로토타입의 계층적인 구조에서 객체의 프로퍼티를 검색한다. 따라서 **프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘**이라고 할 수 있다.
-   이에 반해, 프로퍼티가 아닌 식별자는 스코프 체인에서 검색한다. 자바스크립트 엔진은 함수의 중첩관계로 이루어진 스코프의 계층적 구조에서 식별자를 검색한다. 따라서 **스코프 체인은 식별자 검색을 위한 메커니즘**이라고 할 수 있다.

```jsx
me.hasOwnProperty("name");
```

-   이 예제의 경우, 먼저 스코프 체인에서 me 식별자를 검색한다. me 식별자는 전역에서 선언되었으므로 전역 스코프에서 검색된다. me 식별자를 검색한 다음, me 객체의 프로토타입 체인에서 hasOwnProperty 메서드를 검색한다.

-   이처럼 **스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용된다.**

### 19.8 오버라이딩과 프로퍼티 섀도잉

```jsx
const Person = (function () {
    // 생성자 함수
    function Person(name) {
        this.name = name;
    }

    // 프로토타입 메서드
    Person.prototype.sayHello = function () {
        console.log(`Hi! My name is ${this.name}`);
    };

    // 생성자 함수를 반환
    return Person;
})();

const me = new Person("Lee");

// 인스턴스 메서드
me.sayHello = function () {
    console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello(); // Hey! My name is Lee
```

-   프로토타입이 소유한 프로퍼티(메서드 포함)를 프로토타입 프로퍼티, 인스턴스가 소유한 프로퍼티를 인스턴스 프로퍼티라고 부른다.

-   프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가한다.
-   이때 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩했고 프로토타입 메서드 sayHello는 가려진다. 이처럼 상속 관계에 의해 프로퍼티가 가려지는 현상을 프로퍼티 섀도잉이라 한다.

\*오버라이딩: 상위 클래스가 가지고 잇는 매서드를 하위 클래스가 재정의하여 사용하는 방식

\*오버로딩: 함수 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식 (자바스크립트는 오버로딩을 지원하지 않지만 arguments 객체를 사용하여 구현할 수는 있다.)

-   하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능하다. 다시 말해 하위 객체를 통해 프로토타입에 get 엑세스는 허용되나 set 엑세스는 허용되지 않는다.
    -   따라서 프로토타입 프로퍼티를 변경/삭제하려면 프로토타입에 직접 접근해야 한다.

```jsx
// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
    console.log(`Hey! My name is ${this.name}`);
};
me.sayHello(); // Hey! My name is Lee

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello(); // TypeError: me.sayHello is not a function
```

### 19.9 프로토타입의 교체

-   프로토타입은 임의의 다른 객체로 변경할 수 있다. 이것은 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미한다. 이러한 특징을 활용하여 객체 간의 상속 관계를 동적으로 변경할 수 있다.
    -   프로토타입은 생성자 함수 또는 인스턴스에 의해 교체할 수 있다.

### 19.9.1 생성자 함수에 의한 프로토타입의 교체

```jsx
const Person = (function () {
    function Person(name) {
        this.name = name;
    }

    // 1. 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
    Person.prototype = {
        sayHello() {
            console.log(`Hi! My name is ${this.name}`);
        },
    };

    return Person;
})();

const me = new Person("Lee");
```

-   1에서 Person.prototype에 객체 리터럴을 할당했다. 이는 Person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체한 것이다.

![스크린샷 2023-02-01 오후 5.54.16.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1cb5f657-27c9-4fcf-bfcd-a9915bf2dc6e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-02-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.54.16.png)

-   프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없다. constructor 프로퍼티는 자바스크립트 엔진이 프로토타입을 생성할 때 암묵적으로 추가한 프로퍼티다. 따라서 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

```jsx
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

-   프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하여 프로토타입의 constructor 프로퍼티를 되살림으로서 constructor 프로퍼티와 생성자 함수 간의 연결을 되살릴 수 있다.

```jsx
const Person = (function () {
    function Person(name) {
        this.name = name;
    }

    // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
    Person.prototype = {
        // constructor 프로퍼티와 생성자 함수 감의 연결을 설정
        constructor: Person,
        sayHello() {
            console.log(`Hi! My name is ${this.name}`);
        },
    };

    return Person;
})();

const me = new Person("Lee");

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```

### 19.9.2 인스턴스에 의한 프로토타입의 교체

-   프로토타입은 생성자 함수의 prototype 프로퍼티뿐만 아니라 인스턴스의 ** proto ** 접근자 프로퍼티(또는 Object.getPrototypeOf 메서드)를 통해 접근할 수 있다. 따라서 인스턴스의 ** proto ** 접근자 프로퍼티(또는 Object.getPrototypeOf 메서드)를 통해 프로퍼티 값을 교체할 수 있다.

-   ** proto ** 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것이다.

```jsx
function Person(name) {
    this.name = name;
}

// 프로토타입으로 교체할 객체
const parent = {
    sayHello() {
        console.log(`Hi! My name is ${this.name}`);
    },
};

// 1. me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee
```

![스크린샷 2023-02-01 오후 6.16.34.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7da6c878-e39b-4389-ada2-e177c8adea36/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-02-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.16.34.png)

-   프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없으므로 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
    -   따라서 프로토타입의 constructor 프로퍼티로 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

![스크린샷 2023-02-01 오후 6.18.45.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44b1d5cd-14db-4038-b328-e51e1f0e3131/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-02-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.18.45.png)

-   프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하고 생성자 함수의 prototype 프로퍼티를 재설정하여 파괴된 생성자 함수와 프로토타입 간의 연결을 되살려 보자

```jsx
function Person(name) {
    this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
        console.log(`Hi! My name is ${this.name}`);
    },
};

// 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결을 설정
Person.prototype = parent;
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false

// 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리킨다.
console.log(Person.prototype === Object.getPrototypeOf(me)); // true
```

-   이처럼 프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 꽤나 번거롭다. 따라서 프로토타입은 직접 교체하지 않는 것이 좋다,
    -   상속관계를 인위적으로 설정하려면 19.11절 ‘직접 상속’에서 살펴볼 직접 상속이 더 편리하고 안전하다.
    -   또는 ES6에서 도입된 클래스를 사용하면 간편하고 직관적으로 상속관계를 구현할 수 있다.

### 19.10 instanceof 연산자

-   instanceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받는다.

    -   만약 우변의 피연산자가 함수가 아닌 경우 TypeError가 발생한다.

-   **우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가된다.**

```jsx
// 생성자 함수
function Person(name) {
    this.name = name;
}

const me = new Person("Lee");

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Person); // true

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

```jsx
// 생성자 함수
function Person(name) {
    this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {};

// 프로토타입의 교체
Object.setPrototypeOf(me, parent);

// Person 생성자 함수와 parent 객체는 연결되어 있지 않다.
console.log(Person.prototype === parent); // false
console.log(parent.constructor === Person); // false

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하지 않기 때문에 false로 평가된다.
console.log(me instanceof Person); // false

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true

// parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩한다.
Person.prototype = parent;

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Person); // true

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

-   me 객체는 비록 프로토타입이 교체되어 프로토타입과 생성자 함수 간의 연결이 파괴되었지만 person 생성자 함수에 의해 생성된 인스턴스임에는 틀림이 없다. 그러나 me instanceof Person은 false로 평가된다.
-   이는 Person.prototype이 me 객체의 프로토타입 체인 상에 존재하지 않기 때문이다. 따라서 프로토타이으로 교체한 parent 객체를 Person 생성자 함수의 prototype에 바인딩하면 me instanceof Person은 true로 평가될 것이다.

-   이처럼 instanceof 연산자는 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라 **생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.**

![스크린샷 2023-02-01 오후 10.37.25.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c9add4d-ec8d-4788-88b5-44711566929e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-02-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.37.25.png)

-   me instance of Person의 경우 me 객체의 프로토타입 체인 상에 Person.prototype에 바인딩된 객체가 존재하는지 확인한다.
-   me instance of Object의 경우도 마찬가지다. me 객체의 프로토타입 체인 상에 Object.prototype에 바인딩된 객체가 존재하는지 확인한다.

-   따라서 생성자 함수에 의해 프로토타입이 교체되어 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴되어도 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결은 파괴되지 않으므로 instanceof는 아무런 영향을 받지 않는다.

### 19.11 직접 상속

### 19.11.1 Object.create에 의한 직접 상속

-   Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다. Object.create 메서드도 다른 객체 생성 방식과 마찬가지로 추상연산 OrdinaryObjectCreate를 호출한다.

-   Object.create 메서드의 첫 번째 매개변수에는 생성할 객체의 프로토타입으로 지정할 객체를 전달한다. 두 번째 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달한다.
    -   두 번째 인수는 옵션이므로 생략이 가능하다

```jsx
// 프로토타입이 null인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj -> null
let obj = Object.craete(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지 못한다.
console.log(obj.toString()); // TypeError: obj.toString is not a function

// obj -> Object.prototype -> null
// obj = {}; 과 동일하다.
obj = Object.craete(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj -> Object.prototype -> null
// obj = {x: 1}; 와 동일하다.
obj = Object.create(Object.prototype, {
    x: { value: 1, writable: true, configurable: true },
});
// 위 코드는 아래와 동일하다.
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj -> myProto -> Object.prototype -> null
obj = Object.create(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
    this.name = name;
}

// obj -> Person.prototype -> Object.prototype -> null;
// obj = new Person('Lee')와 동일하다.
obj = Object.create(Person.prototype);
obj.name = "Lee";
console.log(obj.name); // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
```

-   이처럼 Object.create 메서드는 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성한다. 즉, 객체를 생성하면서 직접적으로 구현하는것이다. 이 메서드의 장점은 다음과 같다.

    -   new 연산자 없이도 객체를 생성할 수 있다.
    -   프로토타입을 지정하면서 객체를 생성할 수 있다.
    -   객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

-   그런데 ESLint에서는 앞의 예제와 같이 Object.prototype의 빌트인 메서드를 직접 호출하는 것을 권장하지 않는다
    -   Object.create 메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문이다.
    -   프로토타입 체인의 종점에 위치하는 객체는 Object.prototype의 빌트인 메서드를 사용할 수 없다.

```jsx
// 프로토타입이 null인 객체, 즉 프로토타입 체인의 종점에 위치하는 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

console.log(Object.getPrototypeOf(obj) === null); // true

// obj는 Object.prototype의 빌트인 메서드를 사용할 수 없다.
console.log(obj.hasOwnProperty("a"));
// TypeError: obj.hasOwnProperty is not a function
```

-   따라서 이 같은 에러를 발생시킬 위험을 없애기 위해 Object.prototype의 빌트인 메서드는 다음과 같이 간접적으로 호출하는 것이 좋다.

```jsx
// 프로토타입이 null인 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

// console.log(obj.hasOwnProperty('a'));
// TypeError: obj.hasOwnProperty is not a function

// Object.prototype의 빌트인 메서드는 객체로 직접 호출하지 않는다.
console.log(Object.prototype.hasOwnProperty.call(obj, "a")); // true
```

### 19.11.2 객체 리터럴 내부에서 ** proto ** 에 의한 직접 상속

-   Object.create 메서드에 의한 직접 상속은 앞서 다룬 바와 같이 여러 장점이 있다. 하지만 두 번째 인자로 프로퍼티를 정의하는 것은 번거롭다. 일단 객체를 생성한 이후 프로퍼티를 추가하는 방법도 있으나 이 또한 깔끔한 방법은 아니다.

-   ES6에서는 객체 리터럴 내부에서 ** proto ** 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.

```jsx
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
    y: 20,
    // 객체를 직접 상속받는다.
    // obj -> myProto -> Object.prototype -> null
    __proto__: myProto,
};

/* 위 코드는 아래와 동일하다.
const obj = Object.create(myProto, {
	y: {value: 20, writable: true, enumerable: true, configurable: true}
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototyeOf(obj) === myProto); // true
```

### 19.12 정적 프로퍼티/메서드

-   정적 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말한다.

```jsx
// 생성자 함수
function Person(name) {
    this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
    console.log(`Hi My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = `static prop`;

// 정적 메서드
Person.staticMethod = function () {
    console.log(`staticMethod`);
};

const me = new Person("Lee");

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

-   Person 생성자 함수는 객체이므로 자신의 프로퍼티/메서드를 소유할 수 있다.
-   Person 생성자 함수 객체가 소유한 프로퍼티/메서드를 정적 프로퍼티/메서드라고 한다.
-   정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.

![스크린샷 2023-02-03 오후 2.49.56.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c58d63ef-14c3-4bfe-9509-6949fa9b287b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-02-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.49.56.png)

-   생성자 함수가 생성한 인스턴스는 자신의 프로토타입 체인에 속한 객체의 프로퍼티/메서드에 접근할 수 있다.

-   하지만 정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니므로 인스턴스에 접근할 수 없다.

-   앞에서 살펴본 Object.create 메서드는 Object 생성자 함수의 정적 메서드고 Object.prototype.hasOwnProperty 메서드는 Object.prototype의 메서드다. 따라서 Object.create 메서드는 인스턴스, 즉 Object 생성자 함수가 생성한 객체로 호출할 수 없다. 하지만 Object.prototype.hasOwnProperty 메서드는 모든 객체의 프로토타입의 종점, 즉 Object.prototype이므로 모든 객체가 호출할 수 있다.

```jsx
// Object.create는 정적 메서드다.
const obj = Object.create({ name: "Lee" });

// Object.prototype.hasOwnPrototype는 프로토타입 메서드다.
obj.hasOwnPrototype("name"); // -> false
```

-   만약 인스턴스/프로토타입 메서드 내에서 this를 사용하지 않는다면 그 메서드는 정적 메서드로 변경할 수 있다. 인스턴스가 호출한 인스턴스/프로토타입 메서드 내에서 this는 인스턴스를 기리킨다.
-   매서드 내에서 인스턴스를 참조할 필요가 없다면 정적 메서드로 변경하여도 동작한다. 프로토타입 메서드를 호출하려면 인스턴스를 생성해야하지만 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.

```jsx
function Foo() {}

// 프로토타입 메서드
// this를 참조하지 않는 프로토타입 메서드는 정적 메서드로 변경하여도 동일한 효과를 얻을 수 있다.
Foo.prototype.x = function () {
    console.log("x");
};

const foo = new Foo();
// 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 한다.
foo.x(); // x

// 정적 메서드
Foo.x = function () {
    console.log("x");
};

// 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.
Foo.x(); // x
```

-   참고로 프로토타입 프로퍼티/메서드를 표기할 때 prototype을 #으로 표기(예를 들어. Object.prototype.isPrototypeOf를 Object#isPrototypeOf으로 표기)하는 경우도 있으니 알아두도록 하자.

### 19.13 프로퍼티 존재 확인

### 19.13.1 in 연산자

-   in 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.

```jsx
const person = {
    name: "Lee",
    address: "Seoul",
};

// person 객체에 name 프로퍼티가 존재한다.
console.log("name" in person); // true
// person 객체에 address 프로퍼티가 존재한다.
console.log("address" in person); // true
// person 객체에 age 프로퍼티가 존재하지 않는다.
console.log("age" in person); // false
```

-   in 연산자는 확인 대상 객체의 프로퍼티뿐만 아니라 확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인하므로 주의가 필요하다. person 객체에는 toString이라는 프로퍼티가 없지만 다음 코드의 실행결과는 true다.

```jsx
console.log("toString" in person); // true
```

-   이는 in 연산자가 person객체가 속한 프로토타입 체인 상에 존재하는 모든 프로토타입에서 toString 프로퍼티를 검색했기 때문이다. toString은 Object.prototype의 메서드다.

-   in 연산자 대신 ES6에서 도입된 Reflect.has 메서드를 사용할 수도 있다. Reflect.has 메서드는 in 연산자와 동일하게 동작한다.

```jsx
const person = { name: "Lee" };

console.log(Reflect.has(person, "name")); // true
```

### 19.13.2 Object.prototype.hasOwnProperty 메서드

-   Object.prototype.hasOwnProperty 메서드를 사용해도 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다.

```jsx
console.log(person.hasOwnProperty("name")); // true
console.log(person.hasOwnProperty("toString")); // false
```

-   Object.prototype.hasOwnProperty 메서드는 이름에서 알 수 있듯이 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환하고 상속받은 프로토타입의 프로퍼티 키인 경우 false를 반환한다.

### 19.14 프로퍼티 열거

### 19.14.1 for …in 문

-   객체의 모든 프로퍼티를 순회하며 열거하려면 for…in 문을 사용한다.

```jsx
const person = {
    name: "Lee",
    address: "Seoul",
};

// in 연산자는 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
console.log("toString" in person); // true

// for...in 문의 변수 prop에 person 객체의 프로퍼티 키가 할당된다.

// for..in 문도 객체가 상속받은 모든 프로토타입의 프로퍼티를 열거한다.
// 하지만 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않는다.
for (const key in person) {
    console.log(key + ": " + person[key]);
}
// name: Lee
// address: Seoul
```

-   for…in 문은 객체의 프로퍼티 개수만큼 순회하여 for…in 문의 변수 선언문에서 선언한 변수에 프로퍼티 키를 할당한다.

-   위 예제의 경우 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않는다.
-   이는 toString 메서드가 열거할 수 없도록 정의되어 있는 프로퍼티이기 때문이다.
-   다시 말해, Object.prototype.toString 프로퍼티의 어트리뷰트 [[Enumerable]]의 값이 false이기 때문이다. 프로퍼티 어트리뷰트 [[Enumerable]]은 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다.

```jsx
// Object.getOwnPropertyDescriptor 메서드는 프로퍼티 디스크립터 객체를 반환한다.
// 프로퍼티 디스크립터 객체는 프로퍼티 어트리뷰트 정보를 담고 있는 객체다.
console.log(Object.getOwnPrototypeDescriptor(Object.prototype, "toString"));
// {value: f, writable: true, enumerable: false, configurable: true}
```

-   **for…in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거한다.**

    -   for…in 문은 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.
    -   for…in 문은 프로퍼티를 열거할 때 순서를 보장하지 않는다.

-   배열에는 for…in 문을 사용하지 말고 일반적인 for문이나 for…of문 또는 Array.prototype.forEach메서드를 사용하기를 권장한다.
    -   사실 배열도 객체이므로 프로퍼티와 상속받은 프로퍼티가 포함될 수 있다.

```jsx
const arr = [1, 2, 3];
arr.x = 10; // 배열도 객체이므로 프로퍼티를 가질 수 있다.

for (const i in arr) {
    // 프로퍼티 x도 출력된다.
    console.log(arr[i]); // 1 2 3 10
}

// arr.length는 3이다.
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // 1 2 3
}

// forEach 메서드는 요소가 아닌 프로퍼티는 제외한다.
arr.forEach((v) => console.log(v)); // 1 2 3

// for...of는 변수 선언문에서 선언한 변수에 키가 아닌 값을 할당한다.
for (const value of arr) {
    console.log(value); // 1 2 3
}
```

### 19.14.2 Object.keys/values/entries 메서드

-   for…in문은 객체 자신의 고유 프로퍼티뿐 아니라 상속받은 프로퍼티도 열거한다. 따라서 Object.prototype.hasOwnProperty 메서드를 사용하여 객체 자신의 프로퍼티인지 확인하는 추가 처리가 필요하다.

-   객체 자신의 고유 프로퍼티만 열거하기 위해서는 for…in 문을 사용하는 것보다 Object.keys/values/entries 메서드를 사용하는 것을 권장한다.

-   Object.keys 메서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.

```jsx
const person = {
    name: "Lee",
    address: "Seoul",
    __proto__: { age: 20 },
};

console.log(Object.keys(person)); // ["name", "address"]
```

-   ES6에서 도입된 Object.values 메서드는 객체 자신의 열거 가능한 프로퍼티 값을 배열로반환한다.

```jsx
console.log(Object.values(person)); // ["Lee", "Seoul"]
```

-   ES6에서 도입된 Object.entries 메서드는 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환한다.

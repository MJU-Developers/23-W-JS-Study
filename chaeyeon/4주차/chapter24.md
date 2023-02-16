# 24. 클로저

-   클로저는 자바스크립트 고유의 개념이 아니다. 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어(하스켈, 리스프, 스칼라 등)에서 사용되는 중요한 특성이다.

-   클로저는 자바스크립트 고유의 개념이 아니므로 클로저의 정의가 ECMAScript 사양에 등장하지 않는다 MDN에서는 클로저에 대해 다음과 같이 정의한다.
    > 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

```jsx
const x = 1;

function outerFunc() {
    const x = 10;
    function innerFunc() {
        console.log(x); // 10
    }
    innerFunc();
}
outerFunc();
```

-   이때 중첩 함수 innerFunc의 상위 스코프는 외부 함수 outerFunc의 스코프다. 따라서 중첩 함수 innerFunc 내부에서 자신을 포함하고 있는 외부 함수 outerFunc의 x 변수에 접근할 수 있다.

-   만약 innerFunc 함수가 outerFunc 함수의 내부에서 정의된 중첩 함수가 아니라면 innerFunc 함수를 outerFunc 함수의 내부에서 호출한다 하더라도 outerFunc 함수의 변수에 접근할 수 없다.

```jsx
const x = 1;

function outerFunc() {
    const x = 10;
    innerFunc();
}

function innerFunc() {
    console.log(x); // 1
}

outerFunc();
```

-   이 같은 현상이 발생하는 이유는 자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문이다.

### 24.1 렉시컬 스코프

-   자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다. 이를 렉시컬 스코프(정적 스코프)라 한다.

-   **렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다. 이것이 바로 렉시컬 스코프다.**

### 24.2 함수 객체의 내부 슬롯 [[Environment]]

-   렉시컬 스코프가 가능하려면 함수는 자신이 호출되는 환경과는 상관없이 자신이 정의된 환경, 즉 상위 스코프를 기억해야 한다. 이를 위해 **함수는 자신의 내부 슬롯 [[Environment]]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.**

-   다시 말해, 함수 정의가 평가되어 함수 객체를 생성할 때 자신의 정의된 환경에 의해 결정된 상위 스코프의 참조를 함수 객체 자신의 내부 슬롯 [[Environment]]에 저장한다. 이때 자신의 내부 슬롯 [[Environment]]에 저장된 상위 스코프의 참조는 현재 실행 중인 컨텍스트의 렉시컬 환경을 가리킨다.

-   **따라서 함수 객체의 내부 슬롯 [[Environment]]에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프다. 또한 자신이 호출되었을 때 생성된 함수 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장될 참조값이다. 함수 객체는 내부 슬롯 [[Environment]]에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다.**

```jsx
const x = 1;

function foo() {
    const x = 10;

    // 상위 스코프는 함수 정의 환경(위치)에 따라 결정된다.
    // 함수 호출 위치와 상위 스코프는 아무런 관계가 없다.
    bar();
}

// 함수 bar는 자신의 상위 스코프, 즉 전역 렉시컬 환경을 [[Environment]]에 저장하여 기억한다.
function bar() {
    console.log(x);
}

foo();
bar();
```

-   foo 함수와 bar 함수는 모두 전역에서 함수 선언문으로 정의되었다. 따라서 foo 함수와 bar 함수는 모두 전역 코드가 평가되는 시점에 평가되어 함수 객체를 생성하고 전역 객체 window의 메서드가 된다. 이때 생성된 함수 객체의 내부 슬롯 [[Environment]]에는 함수 정의가 평가된 시점, 즉 전역 코드 평가 시점에 실행 중인 실행 컨텍스트의 렉시컬 환경인 전역 렉시컬 환경의 참조가 저장된다.

-   함수 렉시컬 환경의 구성 요소인 외부 렉시컬 환경에 대한 참조에는 함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조가 할당된다.
    -   즉, 함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조는 바로 함수의 상위 스코프를 의미한다. 이것이 바로 함수 정의 위치에 따라 상위 스코프를 결정하는 렉시컬 스코프의 실체다.

### 24.3 클로저와 렉시컬 환경

```jsx
const x = 1;

// 1
function outer() {
    const x = 10;
    const inner = function () {
        console.log(x);
    }; // 2
    return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // 3
innerFunc(); // 10
```

-   outer 함수의 실행이 종료되면 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거된다. 이때 outer 함수의 지역 변수 x와 변수 값 10을 저장하고 있던 outer 함수의 실행 컨텍스트가 제거되었으므로 outer 함수의 지역 변수 x 또한 생명 주기를 마감한다. 따라서 outer 함수의 지역 변수 x는 더는 유효하지 않게 되어 x 변수에 접근할 수 있는 방법은 달리 없어 보인다.

-   그러나 위 코드의 실행 결과는 outer 함수의 지역 변수 x의 값인 10이다.

-   이처럼 **외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 클로저라고 부른다.**

-   클로저에 대한 MDN의 정의를 다시 살펴 보자

> 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

-   위 정의에서 “그 함수가 선언된 렉시컬 환경”이란 함수가 정의된 위치의 스코프, 즉 상위 스코프를 의미하는 실행 컨텍스트의 렉시컬 환경을 말한다.

-   outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다.
-   outer 함수의 렉시컬 환경은 inner 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있고 inner 함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않기 때문이다. 가비지 컬렉터는 누군가가 참조하고 있는 메모리 공간을 함부로 해제하지 않는다.
-   outer 함수가 반환한 Inner 함수를 호출하면 inner 함수의 실행 컨텍스트가 생성되고 실행 컨텍스트 스택에 푸시된다. 그리고 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 객체의 [[Environment]] 내부 슬롯에 저장되어 있는 참조값이 할당된다.

-   이처럼 중첩함수 inner 내부에서는 상위 스코프를 참조할 수 있으므로 상위 스코프의 식별자를 참조할 수 있고 식별자의 값을 변경할 수도 있다.

---

-   자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저다. 하지만 일반적으로 모든 함수를 클로저라고 하지는 않는다.
-   상위 스코프의 어떤 식별자도 참조하지 않는 경우 대부분의 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않는다.
    -   참조하지도 않는 식별자를 기억하는 것은 메모리 낭비이기 때문이다.
-   상위 스코프의 식별자를 참조하는 중첩 함수더라도 외부 함수보다 일찍 소멸될 경우 클로저라고 하지 않는다.
-   **클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.**
-   또한 모던 브라우저는 최적화를 통해 상위 스코프의 식별자 중에서 클로저가 참조하고 있는 식별자만을 기억한다.
-   클로저에 의해 참조되는 상위 스코프의 변수를 **자유 변수**라고 부른다.
    -   “클로저는 자유 변수에 묶여있는 함수”라고 할 수 있다.

### 24.4 클로저의 활용

-   **클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.** 다시 말해, 상태가 의도치 않게 변경되지 않도록 **상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용**하기 위해 사용한다.
-   함수가 호출될 때마다 호출된 횟수를 누적하여 출력하는 카운터를 만들어보자. 이 예제의 호출된 횟수(num)가 바로 안전하게 변경하고 유지해야 할 상태다.

```jsx
// 카운트 상태 변수
let num = 0;

// 카운트 상태 변경 함수
const increase = function () {
    // 카운트 상태를 1만큼 증가시킨다.
    return ++num;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

-   위 코드는 잘 동작하지만 오류를 발생시킬 가능성을 내포하고 있는 좋지 않은 코드다. 그 이유는 위 예제가 바르게 동작하려면 다음의 전제 조건이 지켜져야 하기 때문이다.

1. 카운트 상태(num 변수의 값)는 Increase 함수가 호출되기 전까지 변경되지 않고 유지되어야 한다.
2. 이를 위해 카운트 상태(num 변수의 값)는 Increase 함수만이 변경할 수 있어야 한다.

-   하지만 카운트 상태는 전역 변수를 통해 관리되고 있기 때문에 언제든지 누구나 접근할 수 있고 변경할 수 있다. 이는 의도치 않게 상태가 변경될 수 있다는 것을 의미한다.
-   따라서 카운트 상태를 안전하게 변경하고 유지하기 위해서는 전역 변수 num을 increase 함수의 지역 변수로 바꾸어 의도치 않은 상태 변경을 방지해야 한다.

```jsx
// 카운트 상태 변경 함수
const increase = function () {
    // 카운트 상태 변수
    let num = 0;

    // 카운트 상태를 1만큼 증가시킨다.
    return ++num;
};

// 이전 상태를 유지하지 못한다
console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```

-   이전 상태를 유지할 수 있도록 클로저를 사용해보자

```jsx
// 카운트 상태 변경 함수
const increase = (function () {
    // 카운트 상태 변수
    let num = 0;

    // 클로저
    return function () {
        // 카운트 상태를 1만큼 증가시킨다.
        return ++num;
    };
})();

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

-   위 코드가 실행되면 즉시 실행 함수가 호출되고 즉시 실행 함수가 반환한 함수가 increase 변수에 할당된다. increase 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저다.

-   즉시 실행 함수는 호출된 이후 소멸되지만 즉시 실행 함수가 반환한 클로저는 increase 변수에 할당되어 호출된다.

-   즉시 실행 함수는 한 번만 실행되므로 increase가 호출될 때마다 num 변수가 재차 초기화될 일을 없을 것이다. 또한 num 변수는 외부에서 직접 접근할 수 없는 은닉된 private 변수이므로 전역 변수를 사용했을 때와 같이 의도하지 않은 변경을 걱정할 필요도 없기 때문에 더 안정적인 프로그래밍이 가능하다.

-   **이처럼 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.**

-   앞선 예제는 카운트 상태를 증가시킬 수만 있다. 다른 예시를 보자

```jsx
const counter = (function () {
    // 카운트 상태 변수
    let num = 0;

    // 클로저인 메서드를 갖는 객체를 반환한다.
    // 객체 리터럴은 스코프를 만들지 않는다.
    // 따라서 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경이다.
    return {
        // num: 0, // 프로퍼티는 public이므로 은닉되지 않는다.
        increase() {
            return ++num;
        },
        decrease() {
            return num > 0 ? --num : 0;
        },
    };
})();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

-   위 예시를 생성자 함수로 표현하면 다음과 같다.

```jsx
const Counter = (function () {
    // 1 카운트 상태 변수
    let num = 0;

    function Counter() {
        // this.num = 0; // 2 프로퍼티는 public하므로 은닉되지 않는다.
    }

    Counter.prototype.increase = function () {
        return ++num;
    };

    Counter.prototype.decrease = function () {
        return num > 0 ? --num : 0;
    };

    return Counter;
})();
```

-   위 예제의 num은 생성자 함수 Counter가 생성할 인스턴스의 프로퍼티가 아니라 즉시 실행 함수 내에서 선언된 변수다.
    -   만약 num이 생성자 함수 Counter가 생성할 인스턴스의 프로퍼티라면 인스턴스를 통해 외부에서 접근이 자유로운 public 프로퍼티가 된다. 하지만 즉시 실행 함수 내에서 선언된 num 변수는 인스턴스를 통해 접근할 수 없으며, 즉시 실행 함수 외부에서도 접근할 수 없는 은닉된 변수다.
-   생성자 함수 Counter는 프로토타입을 통해 increase, decrease 메서드를 상속받는 인스턴스를 생성한다.

-   변수 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될 수 있다. 외부 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍엥서 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 사용된다.
-   다음은 함수형 프로그래밍에서 클로저를 활용하는 간단한 예제다.

```jsx
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
function makeCounter(aux) {
    // 카운트 상태를 유지하기 위한 자유 변수
    let counter = 0;

    // 클로저를 반환
    return function () {
        // 인수로 전달받은 보조 함수에 상태 변경을 위임한다.
        counter = aux(counter);
        return counter;
    };
}

// 보조 함수
function increase(n) {
    return ++n;
}
// 보조 함수
function decrease(n) {
    return --n;
}

// 함수로 함수를 생성한다.
// makeCounter 함수는 보조 함수를 인수로 전달 받아 함수를 반환한다.
const increaser = makeCounter(increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다.
const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

-   makeCounter 함수가 반환하는 하는 함수는 자신이 생성됐을 때의 렉시컬 환경인 makeCounter 함수의 스코프에 속한 counter 변수를 기억하는 클로저다.
-   makeCounter 함수는 인자로 전달받은 보조 함수를 합성하여 자신이 반환하는 함수의 동작을 변경할 수 있다. 이때 주의해야 할 것은 **makeCounter 함수를 호출해 함수를 반환할 때 함수는 자신만의 독립된 렉시컬 환경을 갖는다는 것이다.**

-   makeCounter 함수를 호출하면 새로운 makeCounter 함수의 실행 컨텍스트가 생성된다. 그리고 makeCounter 함수는 함수 객체를 생성하여 반환한 후 소멸된다. makeCounter 함수가 반환한 함수는 makeCounter 함수의 렉시컬 환경을 상위 스코프로서 기억하는 클로저이며, 전역 변수인 decreaser에 할당된다. 이때 makeCounter 함수의 실행 컨텍스트는 소멸되지만 makeCounter 함수 실행 컨텍스트의 렉시컬 환경은 makeCounter 함수가 반환한 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있기 때문에 소멸되지 않는다.

-   전역 변수 increaser와 decreaser에 할당된 함수는 각각 자신만의 독립된 렉시컬 환경을 갖기 때문에 카운트를 유지하기 위한 자유 변수 counter를 공유하지 않아 카운터의 증감이 연동되지 않는다.
-   따라서 독립된 카운터가 아니라 연동하여 증감이 가능한 카운터를 만들려면 렉시컬 환경을 공유하는 클로저를 만들어야 한다. 이를 위해서는 makeCounter 함수를 두 번 호출하지 말아야 한다.

```jsx
// 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
const counter = (function () {
    // 카운트 상태를 유지하기 위한 자유 변수
    let counter = 0;

    // 함수를 인수로 전달받는 클로저를 반환
    return function (aux) {
        // 인수로 전달받은 보조 함수에 상태 변경을 위임한다.
        counter = aux(counter);
        return counter;
    };
})();

// 보조 함수
function increase(n) {
    return ++n;
}

// 보조 함수
function decrease(n) {
    return --n;
}

// 보조 함수를 전달하여 호출
console.log(counter(increase)); // 1
console.log(counter(increase)); // 2
// 자유 변수를 공유한다.
console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0
```

### 24.5 캡슐화와 정보 은닉

-   캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다. 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라 한다.

-   정보 은닉은 외부에 공개할 필요가 없는 구현의 일부를 외부에 공개되지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호하고, 객체 간의 상호 의존성, 즉 결합도를 낮추는 효과가 있다.

-   자바스크립트는 public, private, protected 같은 접근 제한자를 제공하지 않는다. 따라서 자바스크립트 객체의 모든 프로퍼티와 메서드는 기본적으로 외부에 공개되어 있다. 즉, 객체의 모든 프로퍼티와 메서드는 기본적으로 public 하다.

```jsx
function Person(name, age) {
    this.name = name; // public
    let _age = age; // private

    // 인스턴스 메서드
    this.sayHi = function () {
        console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
    };
}

const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined
```

-   name 프로퍼티는 public 하다. 하지만 \_age 변수는 Person 생성자 함수의 지역 변수이므로 Person 생성자 함수 외부에서 참조하거나 변경할 수 없다. 즉, age 변수는 private하다.
-   하지만 위 예제의 sayHi 메서드는 인스턴스 메서드이므로 Person 객체가 생성될 때마다 중복 생성된다. sayHi 메서드를 프로토타입 메서드로 변경하여 sayHi 메서드의 중복 생성을 방지해 보자.

```jsx
function Person(name, age) {
    this.name = name; // public
    let _age = age; // private
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
    // Person 생성자 함수의 지역 변수 _age를 참조할 수 없다.
    console.log(`Hi! My name is ${this.name}. I am ${_age}`);
};
```

-   이때 Person.prototype.sayHi 메서드 내에서 Person 생성자 함수의 지역 변수 \_age를 참조할 수 없는 문제가 발생한다. 따라서 다음과 같이 즉시 실행 함수를 사용하여 Person 생성자 함수와 Person.prototype.sayHi 메서드를 하나의 함수 내에 모아 보자.

```jsx
const Person = (function () {
    let _age = 0; // private

    // 생성자 함수
    function Person(name, age) {
        this.name = name; // public
        _age = age;
    }

    // 프로토타입 메서드
    Person.prototype.sayHi = function () {
        console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
    };

    // 생성자 함수를 반환
    return Person;
})();

const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined
```

-   하지만 위 코드도 문제가 있다. Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 다음과 같이 여러 개의 인스턴스를 생성할 경우 다음과 같이 \_age 변수의 상태가 유지되지 않는다는 것이다.

```jsx
const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person("Kim", 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경된다!
me.sayHi(); // Hi! My name is Lee. I am 30.
```

-   이는 Person.prototype.sayHi 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상이다.
-   Person 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 Person.prototype.sayHi 메서드의 상위 스코프는 어떤 인스턴스로 호출하더라도 하나의 동일한 상위 스코프를 사용하게 된다.
-   이러한 이유로 Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 위와 같이 \_age 변수의 상태가 유지되지 않는다.

-   이처럼 자바스크립트는 정보 은닉을 완벽하게 지원하지 않는다. 인스턴스 메서드를 사용한다면 자유 변수를 통해 private를 흉내 낼 수는 있지만 프로토타입 메서드를 사용한다면 이마저도 불가능해진다.
-   다행히도 2021년부터 TC39 프로세스의 stage 3에는 클래스에 private 필드를 정의할 수 있는 새로운 표준 사양이 제안되어 있다. 이에 대해서는 25절 “private 필드 정의 제안”에서 살펴본다.

### 24.6 자주 발생하는 실수

-   아래는 클로저를 사용할 때 자주 발생할 수 있는 실수를 보여주는 예제다.

```jsx
var funcs = [];

for (var i = 0; i < 3; i++) {
    funcs[i] = function () {
        return i;
    }; // 1
}

for (var j = 0; j < funcs.length; j++) {
    console.log(funcs[j]()); // 2
}
```

-   for 문의 변수 선언문에서 var 키워드로 선언한 i 변수는 블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문에 전역 변수다. 전역 변수 i 에는 0, 1, 2가 순차적으로 할당된다. 따라서 funcs 배열의 요소로 추가한 함수를 호출하면 전역 변수 i를 참조하여 i의 값 3이 출력된다.

-   클로저를 사용해 위 예제를 바르게 동작하는 코드로 만들어보자

```jsx
var funcs = [];

for (var i = 0; i < 3; i++) {
    funcs[i] = (function (id) {
        // 1
        return function () {
            return id;
        };
    })(i);
}

for (var j = 0; j < funcs.length; j++) {
    console.log(funcs[j]());
}
```

-   1 에서 즉시 실행 함수는 전역 변수 i에 현재 할당되어 있는 값을 인수로 전달받아 매개변수 id에 할당한 후 중첩 함수를 반환하고 종료된다. 즉시 실행 함수가 반환한 함수는 funcs 배열에 순차적으로 저장된다.
-   위 예제는 자바스크립트의 함수 레벨 스코프 특성으로 인해 for 문의 선언문에서 var 키워드로 선언한 변수가 전역 변수가 되기 때문에 발생하는 현상이다. ES6의 let 키워드를 사용하면 이 같은 번거로움이 깔끔하게 해결된다.

```jsx
const funcs = [];

for (let i = 0; i < 3; i++) {
    funcs[i] = function () {
        return i;
    };
}

for (let i = 0; i < funcs.length; i++) {
    console.log(funcs[i]()); // 0 1 2
}
```

-   for 문의 변수 선언문에서 let 키워드로 선언한 변수를 사용하면 for 문의 코드 블록이 반복 실행될 때마다 for 문 코드 블록의 새로운 렉시컬 환경이 생성된다. 만약 for 문의 코드 블록 내에서 정의한 함수가 있다면 이 함수의 상위 스코프는 for 문의 코드 블록이 반복 실행될 때마다 생성된 for 문 코드 블록의 새로운 렉시컬 환경이다.
-   이때 함수의 상위 스코프는 for 문의 코드 블록이 반복 실행될 때마다 식별자의 값을 유지해야 한다. 이를 위해 for 문이 반복될 때마다 독립적인 렉시컬 환경을 생성하여 식별자의 값을 유지한다.

-   이처럼 let 이나 const 키워드를 사용하는 반복문은 코드 블록을 반복 실행할 때마다 새로운 렉시컬 환경을 생성하여 반복할 당시의 상태를 마치 스냅숏을 찍는 것 처럼 저장한다.
-   단, 이는 반복문의 코드 블록 내부에서 함수를 정의할 때 의미가 있다. 반복문의 코드 블록 내부에 함수 정의가 없는 반복문이 생성하는 새로운 렉시컬 환경은 반복 직후, 아무도 참조하지 않기 때문에 가비지 컬렉션의 대상이 된다.

-   또 다른 방법으로 함수현 프로그래밍 기법인 고차 함수를 사용하는 방법이 있다. 이 방법은 변수와 반복문의 사용을 억제할 수 있기 때문에 오류를 줄이고 가독성을 좋게 만든다.

```jsx
// 요소가 3개인 배열을 생성하고 배열의 인덱스를 반환하는 함수를 요소로 추가한다.
// 배열의 요소로 추가된 함수들은 모두 클로저다.
const funcs = Array.from(new Array(3), (_, i) => () => i);

// 배열의 요소로 추가된 함수들을 순차적으로 호출한다.
funcs.forEach((f) => console.log(f())); // 0 1 2
```

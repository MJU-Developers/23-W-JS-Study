# Chapter.22 this

---

> **`this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수(self-referencing variable)다.
> `this`를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.**

**_`this`가 가리키는 값, 즉 `this` 바인딩은 함수 호출 방식에 의해 동적으로 결정된다._**

# \***\*함수 호출 방식과 this 바인딩\*\***

---

> **`this`** 바인딩 ( **`this`** 에 바인딩 될 값)은 함수 호출 방식 ,**즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정 된다.**

동일한 함수도 다양한 방식으로 호출 할 수 있다.

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- **`function.prototype.apply`**/**`call`**/**`blind`** 메서드에 의한 간접 호출

```jsx
const foo = function () {
  console.dir(this);
};

///1. 일반함수 호출
///this 는 전역 객체 window 를 가리킨다.

foo(); ///window

///2.메서드에 호출
///foo함수를 프로퍼티 값으로 할당하여 호출
///foo함수 내부의 this 는 메서드를 호출한 obj 를 가리킨다

const obj = { foo };
obj.foo(); //obj

///3.생성자 함수 호출
//foo함수를 new연산자와 함계 생성자 함수로 호출
//foo함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.

new foo(); //foo {}
```

## 일반 함수 호출

---

> **기본적으로 `this` 에는 전역객체가 바인딩 된다.**

```jsx
function foo() {
	console.log("foo's this: ", this); // window
	function bar() {
		console.log("bar's this: " this); // window
	}
	bar();
}
foo();
```

> 위 예제처럼 전역 함수는 물론이고 중첩함수를 **일반 함수로 호출하면 함수 내부의 `this`에는 전역객체가 바인딩된다.**

- _'strict mode'가 적용된 일반 함수 내부의 this에는 undefined가 바인딩 된다._
- _메서드 내에서 정의한 중첩함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역객체가 바인딩 된다._

```jsx
//전역객체 프로퍼티
var value = 1;

const obj = {
  value : 100

  foo() {
  console.log("foo's this:" , this)
    // {value :100 , foo:f}
  console.log("foo's this.value ,this.value)
              //100

              //메서드 내에서 정의한 중첩함수.
  function bar() {
  	console.log("bar's this:",this)
    //window
    console.log("bar's this.value:",this.value)
    //1
    }
  bar()
   }
  }
  obj.foo()
```

> 콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 `**this**`에도 전역객체가 바인딩 된다. 
> **어떠한 함수라도 일반함수로 호출 되면 `this`에 전역객체가 바인딩 된다.**

_이처럼 일반 함수로 호출된 모든 함수(중첩함수 , 콜백함수 포함 ) 내부의 `**this**`는 전역객체가 바인딩 된다._

> 메서드 내부의 중첩 함수나 콜백 함수의 **`this`** 바인딩을 메서드의 `**this**`바인딩과 일치 시키기 위한 방법은 다음과 같다.

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    ///this 바인딩 (this) 변수 that에 할당한다.
    const that = this;

    //콜백함수 내부에서 this 대신 that을 참조한다.

    setTimeout(function () {
      console.log(that.value); ///100
    }, 100);
  },
};
obj.foo();
```

> 이 방법 외에도 `**화살표 함수**`를 이용하는 방법 ,**`apply()`** ,**`call()`**, **`bind()`**를 이용하는 방법으로도 **`this`** 바인딩을 일치 시킬수 있다.

```jsx
var value = 1;
const obj = {
  value: 100,
  foo() {
    //화살표 함수 내부의 this 는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100); //100
  },
};

obj.foo();
```

## 메서드 호출

---

> **메서드 내부의 `this`에는 메서드를 호출한 객체** , 즉 메서드를 호출할때 메서드 이름 앞의 마침표(`.`)연산자 앞에 기술한 **객체가 바인딩 된다**.

```jsx
const person = {
  name: "Lee",

  getName() {
    ///메서드 내부의 this는 메서드를 호출한 객체에 바인딩 된다
    return this.name;
  },
};

//메서드 getName을 호출한 객체는 person 이다.

console.log(person.getName()); //Lee
```

**_`getName` 메서드는 다른객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다._**

```jsx
const anotherPerson = {
  name: "kim",
};
//getName 메서드를  anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

//getName 메서드를 호출한 객체는 anotherPerson이다
console.log(anotherPerson.getName()); //kim

//getName 메서드를 변수에 할당
const getName = person.getName;

//getName메서드를 일반 함수로 호출
console.log(getName()); // ''
//일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
```

> 따라서 메서드 내부에서 사용된 **`this`**는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없고 **메서드를 호출한 객체에 바인딩 된다.**

**_즉, `this`에 바인딩 될 객체는 호출시점에 결정 된다._**

## 생성자 함수 호출

---

> **생성자 함수 내부의 `this`에는 생성자 함수가 생성할 인스턴스가 바인딩 된다.**

- 생성자 함수는 이름 그대로 **객체(인스턴스)를 생성하는 함수**이다.
- 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 **`new`연산자와 함께 호출하면 해당함수는 생성자 함수로 동작한다.**

\*`**new` 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반함수로 동작한다.\*\*\*

```jsx
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person("Lee");

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // Lee

Person.prototype.name = "Kim";

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // Kim
```

```jsx
function Circle(radius) {
  ///생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.redius;
  };
}
///반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
///반지름이 10이 Circle 객체를 생성
const circle2 = new Circlw(10);

console.log(circle1.getDiameter()); //10
console.log(circle2.getDiameter()); //20

//new연자와 함꼐 호출하지 않으면 생성자 함수로 동작하지 않는다 즉 일반젇인 함수의 호출이다

const circle3 = Circle(15);

//일반 함수로 호출된 Circle 에는 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); ///undifined

//일반 함수로 호출된 Circle내부의 this는 전역 객체를 가리킨다.

console.log(radius); //15
```

## function.prototype.apply/call/blind 메서드에 의한 간접 호출

---

> **이들 메서드는 모든 함수가 상속받아 사용할 수 있다.**

### Function.prototype.apply(thisArg[, argsArray])

---

> 주어진 `**this**` 바인딩과 `**인수 리스트 배열**`을 사용하여 **함수를 호출한다.**

- **`thisArg`**: this로 사용할 객체
- **`argsArray`**: 함수에 전달할 인수 리스트의 `**배열 또는 유사배열 객체**`

### Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])

---

> 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 **함수를 호출한다.**

- **`thisArg`**: this로 사용할 객체
- **`argsArray`**: 함수에 전달할 인수 리스트

**_`apply`와 `call`메서드의 본질적인 기능은 함수를 호출하는 것이다. 인수를 전달하는 방식만 다를 뿐 동일하게 동작함._**

### 대표적인 용도

> **`arguments`** 객체와 같은 `**유사 배열 객체**`에 배열 메서드를 사용하는 경우

- 일반 객체에 `**length**` 프로퍼티가 존재하는 경우 `**유사 배열 객체**`라고 말한다.
- **`arguments`** 객체는 함수에 전달된 인수에 해당하는 `**Array**` 형태의 객체이다.
- “`**Array**` 형태"란 `**arguments**`가 `**length**` 속성과 더불어 0부터 인덱스 된 다른 속성을 가지고 있지만, `**Array**`의 `**forEach**`, `**map**`과 같은 내장 메서드를 가지고 있지 않다는 뜻이다.

```jsx
function foo() {
  console.log(arguments);

  const arr = Array.prototype.slice.call(arguments);
  // const arr = Array.prototype.slice.apply(arguments);
  console.log(arr);

  return arr;
}
foo(1, 2, 3); // [1,2,3]
```

### Function.prototype.bind(thisArg)

---

> **`apply`**와 **`call`** 메서드와 다르게 **함수를 호출하지 않는다.**

`**첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.**`

```jsx
function foo() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(foo.bind(thisArg)); // foo
console.log(foo.bind(thisArg)()); // { a:1 }
```

**_`bind` 메서드는 함수를 호출하지 않으므로 명시적으로 호출해야 함._**

### 대표적인 용도

> 메서드의 `**this**`와 메서드 내부의 **중첩 함수 또는 콜백 함수의 `this`가 불일치하는 문제를 해결**하기 위해 사용한다.

```jsx
const person = {
  name: "lee",
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  },
};
person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is lee.
});
```

# 정리
| 함수 호출 방식 | this 바인딩 |
| --- | --- |
| 일반 함수 호출 | 전역 객체 |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달한 객체 |
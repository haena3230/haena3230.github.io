---
title: Javascript es6
categories: [concept]
comments: true
---

# Javascript es6

ECMAScript는 자바스크립트의 표준이며 현재도 계속 ECMA International 표준화 기구에 의해 버전이 업데이트 되고 있다. 그리고 ECMAScript2015를 ES6라고 하며, ES6 이후 버전을 통칭하여 `"ES6+"` 라고 한다.

## 변수

### 선언 및 할당

- 기본적으로는 `const`를 사용한다.
- 재할당이 필요한 경우에만 `let`을 사용한다.
- `var`는 ES2015에서는 쓰지 않는다.

```jsx
//ES5
var A = 1; //변수 선언+할당
var A = 2; //재선언 가능
A = 3; //재할당 가능

console.log(A); //3

//ES6
let B = 2; //변수 선언+할당
let B = 3; //재선언 불가 (Identifier 'B' has already been declared)
B = 4; //재할당 가능

console.log(B); //4

const C = 5; //변수 선언+할당
const C = 6; //재선언 불가(Identifier 'C' has already been declared)
C = 6; //재할당 불가("C" is read-only)

console.log(C); //5
```

### Scope

- `함수 레벨 스코프(Function-level scope)`

함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다. 즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수이다.

```jsx
var a = 10; // 전역변수(global)

function Test() {
  var b = 20; // 지역변수(local)
}

console.log(a); // 10
console.log(b); // "b" is not defined
```

- `블록 레벨 스코프(Block-level scope)`

코드 블록 내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다. 블록의 범위는 중괄호 이며, 함수 스코프 대신 블록 스코프를 사용함으로써 호이스팅 같은 문제도 해결되고, 코드 관리도 수월해졌다.

```jsx
//ES5 var - Function-level scope
if (true) {
  var x = 3;
}

console.log(x); //3

//ES6 let,const - Block-level scope
if (true) {
  const y = 3;
}
console.log(y); //ReferenceError: y is not defined
```

### Hoisting

- 자바스크립트는 모든 선언문(var, let, const, function, function\*, class)이 선언되기 이전에 참조 가능하다.
- `선언 단계(Declaration phase)` - 변수 객체(Variable Object)에 변수를 등록한다. 이 변수 객체는 스코프가 참조하는 대상이 된다
- `초기화 단계(Initialization phase)` - 변수 객체(Variable Object)에 등록된 변수를 메모리에 할당한다. 이 단계에서 변수는 undefined로 초기화된다.
- `할당 단계(Assignment phase)` - undefined로 초기화된 변수에 실제값을 할당한다.

```jsx
//ES5
console.log(foo); // ① undefined
var foo = 123;
console.log(foo); // ② 123
{
  var foo = 456; //global variable
}
console.log(foo); // ③ 456
```

```jsx
//ES6
console.log(foo); // ① ReferenceError: foo is not defined
const foo = 123;
console.log(foo); // ② 123
{
  const foo = 456; //local variable
}
console.log(foo); // ③ 123
```

### Clouser

- 함수와 그 함수가 선언됐을 때의 `렉시컬 환경(Lexical environment)`과의 조합이다.

```jsx
//외부함수(클로저를 생성하는 팩토리 함수다. (객체를 생성하는 공장))
function Outer() {
  const localVar = "I'm local"; // 로컬변수
  //내부함수
  function Inner() {
    //Clouser(내부함수에서 외부함수의 로컬변수로 접근)
    console.log(localVar); // I'm local
  }
  Inner();
}
Outer();
console.log(localVar); //localVar is not defined
```

---

## Template Literals

- 기존에 사용하던 "A"+"B" 문법을 가독성 좋게 변경했다.

```jsx
// ES5
const hello = (name) => {
  console.log(name + "님 안녕하세요!");
};

// Template Literals
const hello = (name) => {
  console.log(`${name}님 안녕하세요!`);
};
```

---

## Object Destructuring (Structuring)

```jsx
// ES5
const person = {
  name: "Master Jung",
  age: 28,
};

const name = person.name;
const age = person.age;
console.log(name, age);

// ES6
const person = {
  name: "Master Jung",
  age: 28,
};

const { name, age: realAge } = person;
console.log(name, realAge);
```

---

## Spread Operators

- 배열과 같은 객체들을 합치기 위해 `unpack을 도와주는것`

```jsx
const beforeNumber = [1, 2, 3, 4];
const afterNumber = [5, 6, 7, 8];
const concatNumber = [...beforeNumber, ...afterNumber];
console.log(concatNumber); // [1,2,3,4,5,6,7,8]
```

---

## Template String

- 그레이브 문자(혹은 backquote) ` 를 이용하여 문자열을 선언하는 방법

---

## Arrow Function

- method에서는 this를 사용할 수 있고, function에서는 this를 사용할 수 없다.

```jsx
// ES5
function A() {
  console.log("test");
}

// ES6
const A = () => {
  console.log("test");
};
```

---

## Class

- class 문법을 지원한다.
- 상속이 가능하다.
- static Method을 사용할 수 있다.
- private method와 private 변수는 사용할 수 없다.

```jsx
// 상속
const Foo = class {
  constructor() {
    this.a = 10;
  }
  getA() {
    return this.a;
  }
  setA(v) {
    this.a = v;
  }
};
const Foo2 = class extends Foo {
  // 상속을 사용할 경우 constructor에 super()를 사용해야합니다.
  // super()는 부모의 생성자(Construcotr) 입니다.
  constructor() {
    super();
  }
  getB() {
    return this.b;
  }
  setB(v) {
    this.b = v;
  }
};
const foo2 = new Foo2();
foo2.setA(10);
foo2.setB(20);
console.log(foo2.getA(), foo2.getB()); // 10 20

// static
const Foo = class {
  constructor() {}
  getName() {
    console.log("this is method of instance");
  }
  static getName() {
    console.log("this is method of Foo Class");
  }
};
var foo = new Foo();
Foo.getName();
foo.getName();
```

---

## 함수

### map

- 함수가 모든 배열을 돌며, 함수의 결과 값들을 저장해서 `새로운 배열` 로 만드는 함수 이다.

### filter

- map과 유사하지만, `참(true)인 값만` 새로운 배열로 작성한다.

### forEach

- 배열의 모든 인자를 실행하지만, map과 filter와 달리 새로운 배열을 리턴(return)하지 않는다.

### push

- 배열의 마지막에 값을 추가한다.

### includes

- 배열에 특정한 값이 있는지 체크한다.

### reverse

- 배열의 앞뒤 값을 바꾼다.

### for in

- for문

```jsx
// ES5
const names = ["jung", "jungKim", "sin"];
for (let index in names) {
  console.log(index, names[index]);
} // 0 jung // 1 jungKim // 2 sin
```

### for of

- for in문과 달리 바로 값을 가져올 수 있다.

```jsx
// ES6
const names = ["jung", "jungKim", "sin"];
for (let name of names) {
  //let 대신 const 사용가능
  console.log(name);
} // jung // jungKim // sin
```

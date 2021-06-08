---
title: Typescript : Type Alias VS Interface
categories: [Javascript$Typescript]
comments: true
---

`typescript`를 통해 프로젝트를 진행하면서, `Type Alias` 와 `interface` 를 활용하는 방법이 헷갈려서 정리 해 두고자 한다.

## Typescript 팀의 의도

TypeScript 팀은 [개방-폐쇄 원칙](https://ko.wikipedia.org/wiki/%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84_%EC%9B%90%EC%B9%99)에 따라 확장에 열려있는 JavaScript 객체의 동작 방식과 비슷하게 연결하도록 `Interface`를 설계했다.

그래서 TypeScript 팀은 가능한 `Type Alias`보단 `Interface`를 사용하고, 합 타입 혹은 튜플 타입을 반드시 써야 되는 상황이면 `Type Alias`를 사용하도록 권장하고 있다.

# 공통점

`Type Alias`와 `Interface` 둘 다 타입에 대해 비슷한 방식으로 이름을 지어줄 수 있다.

```tsx
// interface
interface Human {
  name: string;
  age: number;
}

const henry: Human = {
  name: "남현욱",
  age: 20,
};

// type alias

type Human = {
  name: string;
  age: number;
};

const henry: Human = {
  name: "남현욱",
  age: 20,
};
```

둘다 `Interface`에 대한 `extends`와 `Class`에 대한 `implements` 키워드를 사용하여 관계를 정의할 수 있다.

```tsx
type TBase = {
  t: number;
};

interface IBase {
  i: number;
}

// extends 사용
interface I1 extends TBase {}

interface I2 extends IBase {}

// implements 사용
class C1 implements TBase {
  constructor(public t: number) {}
}

class C2 implements IBase {
  constructor(public i: number) {}
}
```

### 주의

객체 타입이나 객체 타입 간의 `곱 타입`(Intersection Type, 교차 타입), 즉 `정적`으로 모양을 알 수 있는 객체 타입만 동작한다.

→ `합 타입`(Union Type, 결합 타입)은 extends와 implements 대신 다른 키워드로 관계를 정의해야 한다.

### 곱 타입에 대한 사용

```tsx
type TIntersection = TBase & {
  t2: number;
};

interface I3 extends TIntersection {}

class C3 implements TIntersection {
  constructor(public t: number, public t2: number) {}
}

interface I4 extends I3 {}

class C4 implements I3 {
  constructor(public t: number, public t2: number) {}
}
```

### 합 타입에 대한 사용 : 오류

```tsx
// 오류: 합 타입에 대한 extends, implements 사용
type TUnion =
  | TBase
  | {
      u: number;
    };

interface I5 extends TUnion {}

class C5 implements TUnion {}
```

# 차이점

`Interface` : 선언 병합(Declaration Merging)이 가능하다.

→ 동일한 이름으로 여러 번 선언해도 컴파일 시점에 합칠 수 있다,

`Type Alias` : 선언 병합이 불가능하다.

```tsx
interface Window {
  title: string;
}

interface Window {
  ts: import("typescript");
}

declare function getWindow(): Window;

const window = getWindow();
const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {});
```

# 정리

- 가능한 Typescript 팀이 권한하는 방향으로 사용하기.
- 팀 레벨 혹은 프로젝트 레벨에서 지정한 컨벤션에 따라 일관성 있게 사용하기.
- 외부에 공개할 API는 `Interface` 사용하기(for 선언병합)

typescript docs

[Documentation - Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)

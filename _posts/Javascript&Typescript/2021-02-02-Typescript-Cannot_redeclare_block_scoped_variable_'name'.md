---
title: Typescript - Cannot redeclare block-scoped variable 'name'
categories: [Javascript&Typescript]
comments: true
---

# **The problem**

typescript로 프로젝트 진행 중 해당 오류 발생

```
[ts] Cannot redeclare block-scoped variable 'name'.

```

**`name`** 변수 선언시 오류 발생

```
const name = "John";

```

# **The explanation**

TypeScript와 관련된 버그가 아니라 언어의 특징이라고 한다. TypeScript는 기본적으로 글로벌 실행 환경에 DOM 입력을 사용하며 DOM global window 개체에 이름 속성이 있다.

# **The solution**

1. 변수의 이름을 `name` 이 아닌 다른 것으로 바꾼다.
2. TypeScript modules를 사용하고, **`export {};`** 를 추가한다.

```
export {};
const name = "John";

```

3.  컴파일러 옵션을 추가한다. **`tsconfig.json`** 에 **`lib`** 속성을 추가.

```
{
  "compilerOptions": {
    "lib": ["es6"]
  }
}

```

---
title: React Native - Global variables Conflict
categories: [problem]
comments: true
---

- React는 데이터의 흐름이 한 방향으로 흐르고, 컴포넌트를 재사용할 수 있는 특징을 가지고 있다.
- Api call을 통해 받아온 데이터를 사용자가 다룰 수 있도록 구조화 하여 redux를 통해 전역 변수로 지정했다.
- 사용자의 행동에 따라 해당 변수의 상태를update했다.

## The Problem

`Redux` 를 사용한 전역변수를 업데이트하는 과정에서 에러가 발생했다.

## The Explanation

`컴포넌트`를 다양하게 재사용하며 하나의 `전역 변수`를 사용하는 컴포넌트를 여러 방향에서 접근하게 되었다.(다른 페이지에서 생성된 컴포넌트)

### EX

A 페이지에서 접근한 컴포넌트를 없애지 않고 B 페이지에서 컴포넌트를 접근한다.

→ B 컴포넌트의 전역변수를 업데이트 할 시 A 컴포넌트가 제대로 닫히지 않았다면 충돌 발생

→ 전역변수를 참조하는 컴포넌트 생성 시 업데이트는 최소화해야한다.

## The Solution

- Route를 통해 페이지를 넘나드는 변수를 정의하고 관리
- Redux 삭제
- 전역변수 필요시 사용중인 Apollo client 의 client variables활용

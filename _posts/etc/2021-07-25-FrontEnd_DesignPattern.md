---
title: FrontEnd - Design Pattern
categories: [etc]
comments: true
---

# MVC

소프트웨어의 비즈니스 로직과 화면을 구분하는데 중점

### 제작시기

- 초안: 1979.12.10
- 마지막: 2003.08.20

### 특징

- MVC는 Model-View-Controller로 구성됩니다.
- **Model**은 데이터와 비즈니스 로직을 관리합니다.
- **View**는 레이아웃과 화면을 처리합니다.
- **Controller**는 사용자의 입력(Action)을 받고 처리하는 부분입니다.
  - Controller는 여러개의 View를 선택할 수 있는 1:n 구조입니다.

### 동작순서

MVC 패턴의 동작 순서는 아래와 같습니다.

1. 사용자의 Action들은 Controller에 들어오게 됩니다.
2. Controller는 사용자의 Action를 확인하고, Model을 업데이트합니다.
3. Controller는 Model을 나타내줄 View를 선택합니다.
4. View는 Model을 이용하여 화면을 나타냅니다.

### 단점

- MVC 패턴의 단점은 View와 Model 사이의 의존성이 높다는 것입니다. View와 Model의 높은 의존성은 어플리케이션이 커질 수록 복잡하지고 유지보수가 어렵게 만들 수 있습니다.
- 양방향 데이터 흐름을 가지므로, 애플리케이션이 복잡해 진다면, 시스템의 복잡도가 기하급수적으로 증가됩니다.

# MVVM

- 현대 UI 개발 플랫폼에 맞게 제작
- HTML 또는 XAML과 같은 선언적 형태로 항상 수행

### 제작시기

- 초안: 2005.10.08

### 특징

- Model - View - View Model
- 모델-뷰-바인더(model-view-binder) 라고도 합니다.
- **Model**은 사용되는 데이터와 비즈니스 로직을 처리합니다.
- **View**는 사용자에게 보여지는 UI부분입니다.
- **View Model**은 View는 위한 Model입니다.(View를 나타내기 위한 데이터 처리를 하는 부분)
- `Command 패턴` 과 `Data Binding` 두 가지 패턴을 사용했습니다.

  → View와 View Model 사이의 의존성을 없앴습니다.

  → 각각의 부분은 독립적이기 때문에, 모듈화하여 개발이 가능합니다.

### 동작순서

MVVM 패턴의 동작 순서는 아래와 같습니다.

1. 사용자의 Action들은 View를 통해 들어오게 됩니다.
2. View에 Action이 들어오면, Command 패턴으로 View Model에 Action을 전달합니다.
3. View Model은 Model에게 데이터를 요청합니다.
4. Model은 View Model에게 요청받은 데이터를 응답합니다.
5. View Model은 응답 받은 데이터를 가공하여 저장합니다.
6. View는 View Model과 Data Binding하여 화면을 나타냅니다.

### 단점

View Model의 설계가 어렵습니다.

# MVP

기본 MVC 개념을 구성 요소로 분해하고 더욱 세분화하여 프로그래머가 보다 복잡한 애플리케이션을 개발하는 데 도움을 주기 위함입니다.

### 제작시기

- 초안: 2011.06

### 특징

- Model - View - Presenter
- Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분입니다.
- View : 사용자에서 보여지는 UI 부분입니다.
- Presenter : View에서 요청한 정보로 Model을 가공하여 View에 전달해 주는 부분입니다.
  - Presenter는 View와 Model의 인스턴스를 가지고 있어 둘을 연결하는 접착제 역할을 합니다.
  - Presenter와 View는 1:1 관계입니다.

### 동작순서

MVP 패턴의 동작 순서는 아래와 같습니다.

1. 사용자의 Action들은 View를 통해 들어오게 됩니다.
2. View는 데이터를 Presenter에 요청합니다.
3. Presenter는 Model에게 데이터를 요청합니다.
4. Model은 Presenter에서 요청받은 데이터를 응답합니다.
5. Presenter는 View에게 데이터를 응답합니다.
6. View는 Presenter가 응답한 데이터를 이용하여 화면을 나타냅니다.

### 단점

View와 Model으 의존성이 없지만, View와 Presenter 사이의 의존성이 높아졌습니다. 애플리케이션이 복잡해 질수록 View와 Presenter사이의 의존성이 강해집니다.

# Flux

데이터 간의 의존성과 연쇄적인 갱신에서 발생되는 예측하기 힘든 데이터 흐름을 올바르게 다루기 위함입니다. (단방향 데이터 흐름 도입)

### 제작시기

- 초안: 2014.04.30 (f8 2014)

### 특징

- Dispatcher - Store - View로 구성됩니다.
- 각 부분들은 단방향으로만 데이터가 흐르게 됩니다.

  → Dispatcher → Store → View → Action → Dispatcher

- Dispatcher는 Flux의 모든 데이터 흐름을 관리하는 허브 역할을 하는 부분입니다.
  - Action이 발생되면 Dispatcher로 전달되는데, Dispatcher는 전달된 Action을 보고, 등록된 콜백 함수를 실행하여 Store에 데이터를 전달합니다.
  - Dispatcher는 전체 어플리케이션에서 한 개의 인스턴스만 사용됩니다.
- 어플리케이션의 모든 상태 변경은 Store에 의해 결정이 됩니다.
  - Dispatcher로 부터 메시지를 수신 받기 위해서는 Dispatcher에 콜백 함수를 등록해야 합니다.
  - Store가 변경되면 View에 변경되었다는 사실을 알려주게 됩니다.
  - Store은 싱글톤으로 관리됩니다.
- View는 화면에 나타내는 것 뿐만이나라, 자식 View로 데이터를 흘려 보내는 뷰 컨트롤러의 역할도 함께 합니다.
- Dispatcher에서 콜백 함수가 실행 되면 Store가 업데이트 되게 되는데, 이 콜백 함수를 실행 할 떼 데이터가 담겨 있는 객체가 인수로 전달 되어야 합니다. 이 전달 되는 객체를 Action이라고 합니다.

[**참고1]**([**https://peter-cho.gitbook.io/book/5/design-pattern-comparison](https://peter-cho.gitbook.io/book/5/design-pattern-comparison))\*\*

[참고2](<[https://beomy.tistory.com/43](https://beomy.tistory.com/43)>)

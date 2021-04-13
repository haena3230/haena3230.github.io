---
title: React Life Cycle
categories: [concept]
comments: true
---

# React 생명주기

**모든 `컴포넌트`는 다음과 같이 세 가지 단계를 거친다.**

- 초기화단계
- 업데이트단계
- 소멸단계

각 단계에서 몇 개의 메서드들이 정해진 순서대로 되며, 각 단계 속에서 호출되는 메서드를 **생명주기** 메서드라고 부른다.

- **`초기화단계`**는 최초에 컴포넌트 `객체가 생성될 때` 한 번 수행되는 과정이다.
- **`업데이트단계`**는 컴포넌트가 마운트된 이 후, 컴포넌트의 속성값(props), 상태값(state)가 `변경`되면 업데이트 단계가 수행한다.
- **`소멸단계`**는 말 그대로 `소멸`될 때 수행되는 과정이다.

## 생명주기 메서드

UI데이터가 변경되면 자동으로 컴포넌트가 업데이트되며 동적으로 화면을 그려줍니다. 제대로 된 기능을 수행하려면 이런 자동으로 업데이트되는 과정에 끼어들어 API를 호출하기도 하고 데이터를 가공하기도 해야 합니다. 따라서 생명주기의 각 단계별로 필요한 순간에 필요한 작업들을 끼워넣을 수 있는 메서드들이 존재합니다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/156dad0c-7a6d-422f-a83e-e8a927e14d6c/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/156dad0c-7a6d-422f-a83e-e8a927e14d6c/Untitled.png)

생명주기 다이어그램

[React Lifecycle Methods diagram](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

### 초기화 단계

최초에 컴포넌트 객체가 생성될 때 한 번 수행합니다.

- constructor()
- static getDerivedStateFromProps()
- render()
- componentDidMount()

### 업데이트 단계

컴포넌트의 상태나 값이 변경되면 호출되며, 초기화 단계와 소멸 단계 사이에서 반복해서 수행됩니다.

- static getDerivedStateFromProps()
- shouldComponentUpdate()
- render()
- getSnapshotBeforeUpdate()
- componentDidUpdate

### 소멸단계

- componentWillUnmount()

### 렌더링 시 예외 발생

앞의 세 단계와 별개로 렌더링 시에 `예외`가 발생하면 다음 메서드가 호출됩니다.

- static getDerivedStateFromError()
- componentDidCatch()

## constructor

> constructor(props)

constructor 메서드는 보통 초기 속성 값으로부터 상탯값을 만드는 경우에 사용됩니다. 그를 위해서 constructor 메서드 내부에 반드시 super 함수를 호출해야 합니다. super 함수를 호출해야만 React.Component 클래스의 constructor 메서드가 호출됩니다. 따라서 super 함수를 호출하지 않을 시 컴포넌트가 제대로 동작하지 않게 됩니다.

```jsx
class SampleComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      grade: props > 90 ? "A" : "B",
    };
  }
}
```

다른 생명 주기 메서드에서는 상탯값을 변경하기 위해서 setState 메서드를 사용해야 하지만, 유일하게 constructor 메서드에서는 상탯값을 직접 할당할 수 있습니다.

또는 class fields를 사용하면 constructor 메서드를 사용하지 않고 같은 기능을 구현할 수 있습니다.

```jsx
class SampleComponent extends React.Component {
  state = {
    grade: props > 90 ? "A" : "B",
  };
}
```

그런데 흔히 사람들이 하는 실수 중 하나가 constructor 메서드에서 setState 메서드를 호출하는 실수입니다. setState 메서드 호출은 컴포넌트가 마운트 된 이후에만 유효하기 때문에 constructor 메서드 내부에서 호출되는 setState는 무시됩니다.

---

### getDerivedStateFromProps

> static getDerivedStateFromProps(props, state)

getDerivedStateFromProps 메서드는 속성 값을 이용해서 새로운 상탯값을 만들어 낼 때 사용합니다. 이 메서드는 render 메서드가 호출되기 직전에 호출되는데, static 메서드이기 때문에 함수 내부에서 this 객체에 접근할 수 없습니다. getDerivedStateFromProps 메서드는 시간에 따라 변하는 속성 값으로부터 상탯값을 계산하기 위해 추가되었습니다. 애니메이션과 같은 특정 요소의 Y축 위치가가 속성 값일 때 스크롤 여부를 상탯값으로 저장하는 용도로 사용될 수 있습니다.

하지만 매개변수로 들어오는 속성 값에 현재 속성 값은 있지만 비교할 이전 속성 값은 없기 때문에 (없는 것이 좋음) 비교할 이전 속성 값은 상탯값으로 관리해야 합니다.

---

### render

render 메서드는 컴포넌트 작성 시에 반드시 작성해야 하는 메서드입니다. 왜냐하면 화면에 그려질 UI가 render 메서드의 반환 값에 의해 결정되기 때문입니다. render 메서드는 정말 다양한 형태의 반환 값을 수용하는데 그 목록은 아래와 같습니다.

- HTML에 정의된 거의 모든 태그
- 문자열과 숫자
- 배열 (이때 각 리액트 요소는 key 속성 값을 가지고 있어야 합니다.)
- null 또는 bool을 반환하면 아무것도 렌더링 되지 않습니다.
- ReactDOM.createPortal()을 사용하여 현재 위치와 관계없는 특정 돔 요소에 렌더링 할 수 있습니다.

아래의 코드는 null 또는 bool을 반환하면 아무것도 렌더링 하지 않는 render 메서드의 특징을 이용한 코드입니다. title 속성 길이가 0이면 false이기 때문에 아무것도 반환하지 않지만, 1 이상이면 p태그를 리턴하게 됩니다. 이와 같이 bool 리턴을 잘 활용하면 코드의 길이를 많이 줄일 수 있습니다.

```jsx
function SampleComponent({ title }) {
  return title.length > 0 && <p>{title}</p>;
}
```

---

### componentDidMount

componentDidMount 메서드는 render 메서드의 첫 번째 반환 값이 실제 돔에 반영된 직후 호출됩니다. 따라서 render 메서드에서 반환한 리액트 요소가 돔에 반영되어야 알 수 있는 값을 얻을 수 있습니다. 예를 들면 CSS에서 width: 100%와 같은 퍼센트 값들이 실제 돔에서 몇 픽셀로 적용되었는지와 같은 값들을 받아올 수 있습니다.

componentDidMount는 `API 호출을 통해 데이터를 가져올 때 적합`합니다. 왜냐하면 호출한 데이터를 state에 적용하기 위해서는 setState 메서드를 사용해야 하는데 componentDidMount 메서드가 setState 메서드가 동작하는 가장 빠른 시점이기 때문입니다. (컴포넌트가 마운트 된 이후에만 동작하므로)

> 성능이 민감한 애플리케이션이라면 constructor에서 API를 호출하고 프로미스 객체를 이용해 componentDidMount에서 setState로 렌더링 하여 좀 더 빠른 렌더링을 할 수 있지만, 코드가 다소 복잡해지는 단점이 있기 때문에 되도록 componentDidMount 메서드에서 API를 호출하는 것이 좋습니다.

---

### shouldComponentUpdate

> shouldComponentUpdate(nextProps, nextState)

shouldComponentUpdate 메서드는 `성능 최적화`를 위해 존재합니다. 이 메서드는 bool 타입을 반환하는데, 만약 true를 반환하면 render 메서드가 호출되고 false를 반환하면 업데이트 단계는 여기서 멈추게 됩니다. 따라서 개발자는 속성 값과 상탯값을 기반으로 해당 메서드를 작성하게 됩니다. 즉 shouldComponentUpdate 메서드에서 true를 반환하면 render 메서드가 호출되고 가상 돔 수준에서 변경된 내용이 있는지 확인하게 됩니다. 때문에 shouldComponentUpdate에서 변수의 비교를 통해 변경된 내용이 없다면 업데이트 라이프사이클을 끝냄으로써 불필요한 연산을 줄일 수 있습니다.

```jsx
class SampleComponent extends React.Component {
  shouldComponentUpdate(nextProps, nextState) {
    const { age } = this.state;
    return age !== nextState.age;
  }
}
```

위의 예시 코드를 보면 age 상탯값이 변경된 경우에만 참을 반환하므로 나머지 속성 값과 상탯값이 변경되더라도 렌더링 되지 않습니다. 만약 이 메서드를 사용하지 않는다면 항상 true를 반환하기 때문에 실제 돔이 변경되지 않는 상황에서도 항상 가상 돔 비교를 하게 됩니다.

shouldComponentUpdate 메서드는 렌더링 성능 최적화에 필수적이지만, 성급하게 성능을 고려해서 개발할 필요는 없습니다. 먼저 다른 컴포넌트와 부가적인 개발을 끝낸 후 성능 이슈가 발생했을 때 shouldComponentUpdate 메서드를 작성해도 늦지 않습니다. 자세한 내용은 다른 포스팅에서 다루도록 하겠습니다.

---

### getSnapshotBeforeUpdate

> getSnapshotBeforeUpdate(prevProps, prevState) => snapshot

getSnapshotBeforeUpdate 메서드는 렌더링 결과가 실제 돔에 반영되기 직접에 호출됩니다. 따라서 이 메서드를 이용해서 이전 돔의 상탯값을 가져오기 유용합니다. 업데이트 단계에서 실행되는 생명 주기 메서드 중 getSnapshotBeforeUpdate()에서 componentDidUpdate() 메서드 사이에 가상 돔이 실제 돔에 반영됩니다. getSnapshotBeforeUpdate 메서드가 반환한 값 snapshot은 componentDidUpdate 메서드의 세 번째 인자로 들어가데 됩니다. 따라서 getSnapshotBeforeUpdate 메서드가 이전 돔의 상탯값을 반환하면 componentDidUpdate 메서드에서는 돔의 이전 상탯값과 현재 상탯값을 모두 알기 때문에 돔의 상탯값 변화를 감지할 수 있습니다.

---

### componentDidUpdate

> componentDidUpdate(prevProps, prevState, snapshot)

componentDidUpdate는 업데이트 단계에서 가장 마지막에 호출되는 생명 주기 메서드입니다. 가상 돔이 실제 돔이 반영된 직후에 호출되기 때문에 componentDidUpdate 메서드는 새로 반영된 돔의 상탯값을 가장 빠르게 가져올 수 있습니다.

---

### componentWillUnmount

componentWillUnmount 메서드는 소멸 단계에서 호출되는 유일한 메서드입니다. 끝나지 않은 네트워크 요청이나 타이머 해제, 구독 해제 등의 작업을 처리하기 좋습니다. componentDidMount 메서드가 호출되면 componentWillUnmount 메서드도 호출되는 것이 보장됩니다. 그래서 componentDidMount에서 구독하고 componentWillUnmount에서 해제하는 코드가 주로 사용됩니다.

---

### getDerivedStateFromError / componentDidCatch

> static getDerivedStateFromError(error)compoentDidCatch(error, info)

getDerivedStateFromError / componentDidCatch 메서드는 생명 주기 메서드에서 발생한 예외를 처리하는 메서드입니다. 생명 주기 메서드에서 예외가 발생하면 getDerivedStateFromError / componentDidCatch를 구현한 가장 가까운 부모 컴포넌트를 찾게 됩니다.

error 매개변수는 예외가 발생할 때 전달되는 에러 객체이고, info 매개변수는 어떤 컴포넌트에서 에러가 발생했는지를 아려주게 됩니다.

---
title: React Router Dom props
categories: [Web]
comments: true
---

브라우저와 리액트 앱의 Router르 연결하면 Router가 history api에 접근할 수 있게 되고 각 Route와 연결된 컴포넌트의 props로 match, location, history 객체를 기본적으로 전달하게 됩니다. 

## Route와 연결된 컴포넌트의 props

**App.js**

```
import React from "react";
import { Route } from "react-router-dom";
import Home from "./Home";
import About from "./About"

function App() {
  return (
    <div>
      <Route path="/" exact={true} component={Home} />
      <Route path="/about" component={About} />
    </div>
  );
}

export default App;
```

**Home.js**

```
import React from 'react';

const Home = (props) => {
  console.log(props);		// props 출력
  return (
    <div>
      <h1>홈</h1>
      <p>이곳은 홈입니다.</p>
    </div>
  );
}

export default Home;
```

Route와 연결된 Home 컴포넌트의 Props를 확인해보면 `history`, `location`, `match` 객체가 포함되어 있습니다.

## match

match 객체에는 Route path와 URL의 매칭에 대한 정보를 가지고 있습니다.

- **path :** [string] 라우터에 정의된 path
- **url :** [string] 실제 클라이언트로부터 요청된 url path
- **isExact :** [boolean] true일 경우 전체 경로가 완전히 매칭될 경우에만 요청을 수행
- **params :** [JSON object] url path로 전달된 파라미터 객체

## location

location 객체는 현재 페이지에 대한 정보를 가지고 있습니다.

- **pathname** : [string] 현재 페이지의 경로명
- **search** : [string] 현재 페이지의 query string
- **hash** : [string] 현재 페이지의 hash

## history

history 객체는 브라우저의 history api에 접근합니다.

- **length** : [number] 전체 history 스택의 길이
- **action** : [string] 최근에 수행된 action (PUSH, REPLACE or POP)
- **location** : [JSON object] 최근 경로 정보
- **push(path, [state])** : [function] 새로운 경로를 history 스택으로 푸시하여 페이지를 이동
- **replace(path, [state])** : [function] 최근 경로를 history 스택에서 교체하여 페이지를 이동
- **go(n)** : [function] : history 스택의 포인터를 n번째로 이동
- **goBack()** : [function] 이전 페이지로 이동
- **goForward()** : [function] 앞 페이지로 이동
- **block(prompt)** : [function] history 스택의 PUSH/POP 동작을 제어

## typescript에 적용

헤더의 메뉴에서 url에 따라 선택된 메뉴는 다른 스타일을 주도록 했습니다.

```jsx
// route component인 page
import { RouteComponentProps } from 'react-router-dom';
import Footer from '../../Components/Footer/Footer';
import Header from '../../Components/Header';
import { MatchParams } from "../../types";
  
export interface MatchParams {
    pathname: string;
  }

const Brewery = ({ location }: RouteComponentProps<MatchParams>) => {
    const { pathname } = location;
    return (
        <>
        <Header url={pathname}/>
            <div>다섯번째</div>
        <Footer />
        </>
    );
}

export default Brewery
```

```jsx
// header
import {Link} from 'react-router-dom';
import '../../Styles/Header.scss'
interface HeaderProps{
    url:string
}

const Header =({url}:HeaderProps) =>{
    return(
        <header className="header">
            <div className="contents">
                <div className="title">
                    제주맥주
                </div>
                <nav className ="navigation">
                    <ul>
                        <li><Link to="/" className={`${url==='/'?"link_selected":"link"}`} >DIVERSITY</Link></li>
                        <li><Link to ="/supporters" className={`${url==='/supporters'?"link_selected":"link"}`}>SUPPORTERS</Link></li>
                        <li><Link to="/experience" className={`${url==='/experience'?"link_selected":"link"}`}>EXPERIENCE</Link></li>
                        <li><Link to="/shop" className={`${url==='/shop'?"link_selected":"link"}`}>SHOP</Link></li>
                        <li><Link to="/brewery" className={`${url==='/brewery'?"link_selected":"link"}`}>BREWERY</Link></li>
                    </ul>
                </nav>
            </div>
        </header>
    )
}

export default Header
```
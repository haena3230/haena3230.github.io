---
title: React Layout 
categories: [React]
comments: true
---


이전의 프로젝트에서는 주로 컴포넌트 형식으로 사용하는 styled-components를 사용했습니다. scss를 활용하는 방법을 정리했습니다. 

이전에는 React에서 scss를 사용하기 위해서는 next같은 프레임워크를 사용하여 module을 적용하거나, React 프로젝트 자체에 적용하기 위해서 **webpack을 수정해야 했습니다**. 

또한, create-react-app에서 project를 만들경우 처음에는 webpack을 수정할 수 없으니, **eject 명령을 통해 프로젝트를 customize해야했습니다.**

하지만, eject후 webpack의 webpack.config.js설정을 살펴보니, 최근의 react-create-app프로젝트는 webpack 설정 없이 sass를 css로 변환해주는 라이브러리만 설치해주면 사용이 가능했습니다. 

헤더 컴포넌트에서 scss를 적용하는 과정입니다.

## 변환 라이브러리 설치

일반 node-sass설치 후 버전 에러가 발생하여 5.0.0버전으로 설치해주었습니다.

```jsx
yarn add node-sass@5.0.0
```

## tsx컴포넌트 작성

header 컴포넌트에 scss를 적용해보겠습니다. 

```html
// Components/Header

const Header =() =>{
	return(
	<header>
		<div>
			<div>로고입니다</div>
			<nav>
				<ul>
					<li>
					DIVERSITY
					</li>
					<li>
					SUPPORTERS
					</li>
					<li>
					EXPERIENCE
					</li>
					<li>
					SHOP
					</li>
					<li>
					BREWERY
					</li>
				</ul>
			</nav>
		</div>
	</header>
	)
}

export default Header
```

## scss 작성하기

```jsx
// Components/Header.scss

.header {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 80px;
    background-color: #dde0ea;
  }
  
  .contents {
    display: flex;
    width: 96%;
    max-width: 1100px;
    height: 100%;
    margin: 0 auto;
    align-items: center;
    justify-content: space-between;
  }
  
  .navigation {
    ul {
      display: flex;
      list-style: none;
  
      li + li {
        margin-left: 30px;
      }
    }
  }
```

## scss 적용하기

```jsx
import './Header.scss'

const Header =() =>{
    
    return(
        <header className="header">
            <div className="contents">
                ...
            </div>
        </header>
    )
}

export default Header
```

아직 style을 제대로 설정하지 않아 미흡하지만, scss가 잘 적용되는 것을 볼 수 있습니다.

![Untitled](https://user-images.githubusercontent.com/57908055/130592179-c97d9ff6-3900-4978-8b1f-7fce5076849c.png)

## 추가

scss 파일 앞에 _를 붙여준다면, css파일로 컴파일 되지 않고 변수 scss파일로 사용할 수 있습니다.
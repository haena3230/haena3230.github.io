---
title: React Layout 
categories: [React]
comments: true
---

재사용 할 수 있도록 Header와 Footer같은 Layout을 작성하는 방법입니다. 

[다음의 블로그를 참고했습니다.]([https://velog.io/@pumpkin-raccoon/layout-component-by-react](https://velog.io/@pumpkin-raccoon/layout-component-by-react))

## Layout 컴포넌트 생성하기

우선, 원하는 디렉토리에 Layout 컴포넌트를 생성합니다. 저는 TypeScript를 사용했기때문에 tsx파일로 생성했습니다. 

```html
// Pages/index.tsx
const Layout = () => {
  return (
    <div>

    </div>
  )
}

export default Layout
```

저는 레이아웃을 메뉴바를 헤더에 작성하고, 콘텐츠, 푸터 영역으로 나누었습니다.

## Header, Footer 생성

```
// Components/Header/index.tsx
const Header = () => {
  return (
    <header>
      <h2>헤더입니다.</h2>
    </header>
  )
}

export default Header
```

```
// Components/Footer/index.tsx
const Footer = () => {
  return (
    <footer>
      <h2>푸터입니다</h2>
    </footer>
  )
}

export default Footer
```

## Layout 컴포넌트에 적용

그리고 만든 헤더와 푸터를 레이아웃 컴포넌트에 적용했습니다.

```
// Pages/index.tsx
import Footer from "./Components/Footer"
import Header from "./Components/Header"

const Layout = () => {
  return (
    <div>
      <Header />

      <Footer />
    </div>
  )
}

export default Layout
```

## 컴포넌트를 넣을 수 있도록 설정

이후 레이아웃 내부에 페이지에 해당하는 컴포넌트를 넣을 수 있도록 property로 컴포넌트를 받도록 반영했습니다.

```
// Pages/index.tsx
import Footer from "./Components/Footer"
import Header from "./Components/Header"

const Layout = (props: {
  children: React.ReactNode
}) => {
  return (
    <div>
      <Header />

      <main>
        {props.children}
      </main>

      <Footer />
    </div>
  )
}

export default Layout
```

## Layout 컴포넌트 적용하기

```
// Pages/Diversity/index.tsx
import Layout from "../Pages"

const HomePage = () => {
  return (
    <Layout>
      <... />
    </Layout>
  )
}

export default HomePage
```

그러면 이제 다음과 같이 헤더와 푸터가 적용된 것을 확인할 수 있습니다.Layout 컴포넌트 내부의 내용이 children으로 props가 되어 넘어갔습니다.

![Untitled](https://user-images.githubusercontent.com/57908055/130590768-b6ece4b1-ba9a-4c5b-909a-c8c0067b28e5.png)

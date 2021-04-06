---
title: Start Gihub Blog
categories: [etc]
comments: true
---

기존에 정보를 정리 해 두기 위해 [Notion](https://www.notion.so/haena3230/Notes-f286e05dc6cf4c9cb39843c4d15c50a2)을 활용했지만, 단순한 기록 형식에 가까웠던 방식을 오픈하여 소통하기 위한 공간으로 만들기 위해 [Github Blog](https://haena3230.github.io/)를 활용하고자 합니다

시간이 날 때 마다 기록해 두었던 정보들을 통합하고 정리해 갈 예정이며, 블로그 생성 및 관리를 위해 다음과 같은 방법을 활용했습니다.

## Github Blog 생성 방법

Github의 Blog를 생성하고 관리하는 다양한 방법이 있지만, 간단하게 참고하여 시작할 수 있는 방법 위주로 작성했습니다.

### 1. repository 생성

Github에 접속해서 새로운 레파지토리(new repository)를 생성합니다. 이때, 저장소의 이름을 **username}.github.io**형식으로 만들면 잠시 후 해당 url을 통해 Github에서 자동으로 페이지를 업데이트 해 줍니다.

### 2-1. 테마 고르기

직접 깃허브의 테마를 만들어 갈 수 있지만, 미리 만들어진 테마를 쉽게 적용할 수 있습니다. 깃허브의 블로그 테마는 **jekyll**을 활용해 만들어 졌으며, 다음의 테마 관련 사이트들에서 테마를 고를 수 있습니다.

- [http://jekyllthemes.org/](http://jekyllthemes.org/)
- [https://jekyllthemes.io/free](https://jekyllthemes.io/free)
- [https://jamstackthemes.dev/ssg/jekyll/](https://jamstackthemes.dev/ssg/jekyll/)
- [https://jekyll-themes.com/free/](https://jekyll-themes.com/free/)

> 직접 테마를 작성하기 위해서는 [여기의](http://jekyllrb-ko.github.io/docs/step-by-step/01-setup/) 페이지를 참고할 수 있습니다.

### 2-2. 테마 적용하기

마음에 드는 테마가 있다면, 해당 페이지에 있는 테마 파일을 받아와야 합니다. 테마 파일을 적용하기 위한 방법은 다양하여 페이지의 사용방법을 참고해야 합니다. 주로 **다운로드 링크**, **github clone**, **fork** 등의 방법을 사용할 수 있으며, 필자는 [깔끔하고 쉽게 시작할 수 있는](https://github.com/barryclark/jekyll-now) 테마를 선택하여 fork했습니다. 그 후 간단하게 **scss**를 수정하여 스타일을 바꿔줬습니다.

### 3. 세부사항 작성하기

테마 파일 중 **\_config.yml**을 통해 세부 사항을 적용할 수 있습니다. 테마에서 제공하는 사용 방법을 참고하여 세부사항을 작성하도록 합니다.

### 추가 테마 수정, 관리하기

테마에 따라 이후의 관리 과정에 차이가 있습니다. 제가 선택한 테마는 **Jekyll Now**를 사용하여 ruby 개발환경 설정과 같은 단계가 없이도 바로 게시글을 작성하고 수정할 수 있습니다. 테마에 따라 개발 환경이 필요하다면 다음과 같은 단계를 거쳐야 합니다.

**ruby개발환경 설치 -> jekyll bundler 설치 -> 플러그인 설치 -> 로컬 서버 실행 -> 수정 -> 게시**

테마를 설치, 수정, 관리하기 위해서는 **Ruby** 개발 환경이 필요합니다. [ruby download](https://www.ruby-lang.org/ko/downloads/)

```
// 버전 확인을 통한 설치 확인
ruby -v
```

ruby 설치가 완료되었다면 **jekyll과 bundler**를 설치해야 합니다.

```
gem install jekyll bundler
```

테마에 필요한 플러그인을 다운받아야 합니다. 다운로드 받은 파일의 **Gemfile**은 **ruby bundler**의 설정 파일로, 각종 플러그인의 의존성을 관리할 수 있는 파일입니다.

```
bundle install
```

준비가 다 끝났다면, jekyll 로컬 서버를 구동해 **http://127.0.0.1:4000** 에서 수정 작업을 확인하며 진행할 수 있습니다.

```
bundle exec jekyll serve
```

수정 작업 후 Github Repository에 push하면 작업한 결과물이 잠시 후 적용되는 것을 확인할 수 있습니다.

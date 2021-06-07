---
title: CI/CD Github Action
categories: [etc]
comments: true
---

`contestbox` 프로젝트를 진행하면서 협업을 진행하면서 프로젝트의 상태를 확인할 수 있는 다양한 방법이 존재한다는 것과, 작업들을 `자동화` 할 수 있는 CI/CD를 알게 되었다.

현재까지 프로젝트를 진행하면서, apk 를 빌드하고 팀원들과 공유하는 과정이 따로 필요했지만, 그 과정을 자동화 하고 파일을 공유할 수 있는 환경이 있다는 장점을 적용해 보고 싶었다.

무료로 지원해주면서 500MG의 저장 장소를 지원해 주는 `Github Action`을 통해 `master branch`로 `push` 할 시에 `Android apk 파일`을 자동으로 빌드해주는 CI 과정을 진행 해 보았다.

Build 전 Test작업 Note

[Jest(Test)](https://www.notion.so/haena3230/Jest-Test-e97ceb02354942bea7befe04b442e1b5)

## CI : 지속적인 통합

"CI"는 개발자를 위한 자동화 프로세스인 `지속적인 통합(Continuous Integration)`을 의미한다.

CI를 성공적으로 구현할 경우 애플리케이션에 대한 새로운 코드 변경 사항이 정기적으로 `빌드 및 테스트`되어 공유 리포지토리에 통합되므로 여러 명의 개발자가 동시에 애플리케이션 개발과 관련된 코드 작업을 할 경우 서로 충돌할 수 있는 문제를 해결할 수 있다.

## CD : 파이프라인의 추가 단계에 대한 자동화

- 지속적인 서비스 제공(Continuous Delivery)

개발자들이 애플리케이션에 적용한 변경 사항이 `버그 테스트`를 거쳐 `리포지토리`(예: [GitHub](https://redhatofficial.github.io/#!/main) 또는 컨테이너 레지스트리)에 자동으로 `업로드`되는 것을 뜻한다.

`운영팀`은 이 리포지토리에서 애플리케이션을 실시간 프로덕션 환경으로 `배포`할 수 있다.

이는 개발팀과 비즈니스팀 간의 가시성과 커뮤니케이션 부족 문제를 해결해 준다.

지속적인 제공은 최소한의 노력으로 새로운 코드를 배포하는 것을 목표로 한다.

- 지속적인 배포(Continuous Deployment)

개발자의 변경 사항을 `리포지토리`에서 고객이 사용 가능한 `프로덕션 환경`까지 자동으로 릴리스하는 것을 의미한다.

이는 애플리케이션 제공 속도를 저해하는 수동 프로세스로 인한 운영팀의 프로세스 `과부하 문제`를 해결한다.

# Github Action

소프트웨어 workflow를 자동화할 수 있도록 도와주는 도구이다.

- [공식 홈페이지](https://github.com/features/actions), [공식 문서](https://help.github.com/en/actions)

### Workflow의 대표적인 예

1. Test Code

- ex) 특정 함수의 return 값이 어떻게 나오는지 확인하는 테스트 코드
- ex) df의 타입이 pd.DataFrame이 맞는가?
- ex) value1에 특정 값이 들어가는가?
- 쿼리를 날리고 데이터가 맞는지 정합성 체크하는 것도 일종의 테스트

2. 배포

- 서버에 새로운 기능, 버전 등을 배포

3. 기타 자동화하고 싶은 스크립트

- 주기적으로 데이터를 수집해 처리

4. 다양한 파이썬 버전에서 실행되는지 확인

### 가격

- Public repo : 무료, 단 limit이 있음
- Private repo : [링크](https://help.github.com/en/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-actions) 참고

### 사용할 수 있는 한도

- Workflow는 하나의 Repo에 최대 20개까지 등록 가능
- Workflow 안에 존재하는 Job은 6시간동안 실행될 수 있고, 초과시 자동으로 중지됨
- Github Free는 Storage 한도 500MB, 월에 실행 시간 2,000분
  - Github Pro는 Storage 한도 1GB, 월에 실행 시간 3,000분

## **Github Action Core 개념**

- 1. Workflow

  - 여러 Job으로 구성되고, Event에 의해 트리거될 수 있는 자동화된 프로세스
  - 최상위 개념
  - Workflow 파일은 YAML으로 작성되고, Github Repository의 `.github/workflows` 폴더 아래에 저장됨

- 2. Event

  - Workflow를 Trigger(실행)하는 특정 활동이나 규칙
  - 예를 들어 다음과 같은 상황을 사용할 수 있음
    - 특정 브랜치로 Push하거나
    - 특정 브랜치로 Pull Request하거나
    - 특정 시간대에 반복(Cron)
    - Webhook을 사용해 외부 이벤트를 통해 실행
  - 자세한 내용은 [Events that trigger workflows](https://help.github.com/en/actions/reference/events-that-trigger-workflows#external-events-repository_dispatch) 참고

- 3. Job

  - Job은 여러 Step으로 구성되고, 가상 환경의 인스턴스에서 실행됨
  - 다른 Job에 의존 관계를 가질 수 있고, 독립적으로 병렬 실행도 가능함

- 4. Step

  - Task들의 집합으로, 커맨드를 날리거나 action을 실행할 수 있음

- 5. Action

  - Workflow의 가장 작은 블럭(smallest portable building block)
  - Job을 만들기 위해 Step들을 연결할 수 있음
  - 재사용이 가능한 컴포넌트
  - 개인적으로 만든 Action을 사용할 수도 있고, Marketplace에 있는 공용 Action을 사용할 수도 있음
    - [Github Marketplace](https://github.com/marketplace?type=actions)와 [Github Actions Repository](https://github.com/actions/)에서 확인 가능

- 6. Runner
  - Gitbub Action Runner 어플리케이션이 설치된 머신으로, Workflow가 실행될 인스턴스
  - Github에서 호스팅해주는 Github-hosted runner와 직접 호스팅하는 Self-hosted runner로 나뉨
  - Github-hosted runner는 Azure의 Standard_DS2_v2로 vCPU 2, 메모리 7GB, 임시 스토리지 14GB
    - [Self-hosted runners](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/adding-self-hosted-runners) 참고

### 적용

코드 작성 → Workflow 정의 → Test 과정을 통해 생성할 수 있으며, Android apk를 빌드하는 과정을 자동화하는 것이 목적이었기 때문에, 다음의 과정을 통해 자동화했다.

- workflow 생성 : `.github/workflow` 디렉토리, `.yml` 파일 생성
- Event : Master branch에 push할 시 job trigger
- job : apk 파일 생성

Githib Action이 설정 된 레포지토리 링크만 있다면, 아무때나 가장 최근에 작업한 버전의 apk 파일을 다운받을 수 있다.

```jsx
name: AndroidCI

on:
  push:
    branches: [master]
jobs:
  build-android:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install npm dependencies
        run: |
          npm install
      - name: Make gradlew executable
        run: cd android && chmod +x ./gradlew
      - name: Build Android Release
        run: |
          cd android && ./gradlew app:assembleRelease
      - name: Upload Artifact
        uses: actions/upload-artifact@v1
        with:
          name: app-release.apk
          path: android/app/build/outputs/apk/release/
```

[Github Repository](https://github.com/contestbox/android.git)

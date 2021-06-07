---
title: Github Issue 활용법
categories: [etc]
comments: true
---

`contestbox` 프로젝트를 진행하면서 협업 도구로 `notion` 과 `Figma` 를 활용했지만, 프로젝트의 자세한 작업내용, 코드 리뷰 등의 세세한 내용은 github의 issue와 project를 통해 관리하는 것으로 했다.

## **Issue 발행**

Issue는 모든 것이 이슈라고 불릴 수 있다. 새로운 추가될 기능, 개선 해야할 기능, 버그 등등 모든 것이 이슈라고 볼 수 있다. 모든 활동 내역에 대해서 이슈를 등록하고 그 이슈 기반으로 작업을 하게 된다.

우리는 Github의 issue 탭을 사용해서 이를 기반으로 feature 브랜치를 생성해서 작업할 예정이다.

## **Issue Template**

이슈 템플릿을 사용해서 형식을 지정하여 사용할 수 있다. .github/ 디렉토리 밑에 ISSUE_TEMPLATE.md 파일에 템플릿을 지정해서 사용할 수 있다. 우리는 issue 템플릿은 사용하지 않는다.

## **Issue 작업**

다음 예제의 사진을 통해서 등록된 issue를 살펴보자.

![https://woovictory.github.io/img/github_issue.png](https://woovictory.github.io/img/github_issue.png)

- Assignees : 해당 작업의 담당자
- Labels : 해당 작업의 성격을 지정한다.
- Milestone : 해당 작업이 속한 파트

![https://woovictory.github.io/img/github_milestone.png](https://woovictory.github.io/img/github_milestone.png)

다른 것들은 이해하기 쉽지만 Milestone은 생소하다. Milestone은 이번 출시 버전이 1.0.0일 경우 해당 버전이든 이슈(작업) 기능 강화, 새 기능추가, 버그 기타 등등 모든 이슈를 Version 1.0.0 Milestone이라는 항목에 추가하면 위 그림처럼 Version 1.0.0에 대한 전체적인 상황을 한 눈에 볼 수가 있는 장점이 있다.

**1. Issue 생성**

일단, 우리는 먼저 Github의 issue 탭에 들어가서 **기능 정의서**를 기반으로 issue를 생성한다. 이슈는 `스토리_기능`의 형태로 이슈를 생성했다. 기본 기능들을 미리 이슈 탭을 이용해서 기능을 정리한 것이다. 이슈를 생성할 때 자동으로 번호가 발급되는데 이 **번호**를 사용해서 feature 브랜치를 생성할 것이다.

**2. 브랜치 생성**

이제 발급된 이슈 번호를 기준으로 **feature** 브랜치를 생성하면 된다. Git flow에 따라서 develop 브랜치에서 feature 브랜치를 딴다.

```
git checkout -b feature_#033_util_sharedpreference develop

```

위의 명령어는 develop으로부터 feature\_#033_util_sharedpreference라는 브랜치를 생성하고 그 브랜치로 이동하는 것을 의미한다.

**3. 작업 수행 후 commit**

코드를 수정하거나 클래스를 만드는 등의 작업을 수행하고 커밋을 해준다.

```
git commit -a -m "#003_feat_쉐어드 프리퍼런스 구현"

```

위의 명령어는 git add와 git commit을 합친 형태이다. 메시지도 위와 같은 형식을 지켜서 작성한다.

**4. 원격 저장소로 push 작업**

이제 commit도 완료했으니 원격 저장소(origin)으로 push를 하면 된다.

```
git push origin feature_#033_util_sharedpreference

```

**5. PR & Merge**

이제 Merge를 해달라는 Pull Request를 작성한다.

![https://woovictory.github.io/img/github_PR.png](https://woovictory.github.io/img/github_PR.png)

- PR의 이름은 Feature #033 util SharedPreference로 작성한다.
- 내용은 PR Template을 이용하여 개요와 작업 사항, 사용 방법 등을 작성한다.
- 그리고 작성자와 라벨을 달고 리뷰어를 설정한 뒤 PR를 날린다.
- 코드 리뷰를 받는다.
- 수정 사항이 있다면 코드를 수정하고 다시 push를 날린다.
- 여기서 `resolved #이슈번호`로 작성하면 merge될 때 해당 issue는 자동으로 삭제된다.

![https://woovictory.github.io/img/github_PR2.png](https://woovictory.github.io/img/github_PR2.png)

- 그러면 수정 사항에 대해서 링크가 나오고 위와 같이 답글을 달아준다.
- 그리고 코드 리뷰가 완료되고 리뷰어가 approve를 해주면 자신이 merge를 한다.
- 그리고 해당 브랜치는 더는 필요가 없다고 판단되면 Delete branch 버튼을 통해서 Remote에 있는 브랜치 삭제 해주면 된다.

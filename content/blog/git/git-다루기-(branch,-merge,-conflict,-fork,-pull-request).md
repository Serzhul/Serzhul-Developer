---
title: Git 다루기 (Branch, Merge, Conflict, Fork, Pull request)
date: 2021-09-27 12:09:28
category: git
thumbnail: { thumbnailSrc }
draft: false
---

# Branch가 필요한 이유

- commit을 여러번 반복해 버전을 쌓아가는 것은 같은 branch 내에서 작업하는 것을 의미한다.
- 그러나 여러 명이 동시에 작업하는 경우 같은 branch내에서 작업하는 것은 충돌이 날 수 있다. 같은 코드를 동시에 수정할 수 있기 때문이다.
- 따라서 작업할 당시에는 여러 갈래(Branch)로 나눠서 작업하고 나중에 병합(Merge)하는 작업하는 것이 효과적이다. 병합했을 때, 충돌(conflict)이 발생할 수 있는데 이 경우 branch가 나눠져 있기 때문에 최소한의 수정으로 충돌 문제를 해결할 수 있다.
- 현재 작업하고 있는 branch를 head branch라고 하며 새로운 branch에 작업하기 위해선 branch를 만들고 head branch를 새로운 branch로 옮겨줘야 한다.

# Branch 다루기

## 1. git branch

- git bracnh 명령어는 새로운 branch를 생성하는 명령어이다.
- branch 뒤에 이름을 주면 해당 이름을 가진 branch가 생성된다.

```bash
$ git branch 이름
```

## 2. git checkout

- head branch를 변경하는 명령어이다.

```bash
$ git checkout 이름
```

- 해당 branch에서 작업하고 commit과 push를 하게되면 github repository에서 pull request를 확인하고 master(기본 branch)에 merge 할 지 여부를 결정할 수 있다.

## 3. branch 삭제

- 해당 branch에서 더 이상 작업할 필요가 없어졌을 경우 -D 옵션을 통해 local branch를 삭제 할 수 있다.
- 또한 원격 저장소의 branch를 삭제하고자 할 경우 push origin :이름 혹은 —delete 이름으로 삭제할 수 있다.

```bash
$ git branch 이름 -D
$ git push origin :이름 (혹은 git push origin --delete 이름)
```

- 삭제 시 주의할 점은 현재 checkout된 branch는 삭제할 수 없으므로 다른 branch로 변경 후 삭제해야한다는 점이다.
- 또한 변경 사항이 남아있을 경우에 checkout이 불가능하기 때문에 모든 작업을 마치고 checkout 후에 삭제하면 된다.

# Fork

- git 기능 중에 fork 기능이 있다. fork는 저장소를 통째로 복사하는 기능으로, 다른 사람의 저장소를 복제해와서 자유롭게 수정할 수 있도록 할 수 있다.
- 만약 오픈소스 프로젝트에 기여하고 싶다면 Fork를 통해 오픈소스 프로젝트를 복사해오고 로컬에서 수정한 다음 commit-push를 할 수 있다. 이 때, pull request를 통해 merge를 요청할 수 있고 merge가 진행되면 내가 수정한 코드가 해당 프로젝트에 반영될 수 있다.

### branch와 fork 비교

- branch는 하나의 원본저장소에서 분기를 나눈다면, 포크는 여러 원격저장소를 만들어 분기를 나눈다고 볼 수 있다.
- branch는 하나의 원본저장소에서 commit 이력을 편하게 볼 수 있고, fork는 원본 저장소에 영향을 미치지 않으므로 코드를 자유롭게 수정할 수 있다는 장점이 있다.
- branch는 다수의 사용자가 다수의 브랜치를 만들게 되면 복잡해지기 때문에 관리가 어렵다는 단점이 있고, fork는 원본저장소의 이력을 보려면 따로 주소를 추가해줘야 되서 번거로워진다는 단점이 있다.

# Pull Request

- 원본 저장소에 코드를 올릴 권한이 없을 때, merge를 요청하는 개념이라고 할 수 있다.
- merge하고 싶은 두 브랜치를 선택하고, 어떤 변경을 했는지 제목과 내용에 작성 후 단일 저장소 혹은 포크한 저장소에서 보낼 수 있다.
- base는 merge될 저장소, compare는 새롭게 만든 merge 할 저장소 개념이다.
- pull request로 merge 요청 보내기
  - 직접 merge는 피하고 모든 merge 과정을 pull request를 통해 진행한다.
  - 동료가 PR(pull request)를 보고 코드를 리뷰할 수 있다.
  - PR에 수정이 필요하면 comment를 달아 change request를 보낼 수 있다.
  - 오픈소스에 PR을 보낼시 contribution guideline을 참고해서 보내야 한다.

### TIP : 브랜치 관리하기

1. 보통 feat/기능이름 으로 한 사람이 개발하는 기능 브랜치를 만든다. (혹은 fix/버그이름 hotfix/급한버그)
2. 작업이 끝나면 dev (혹은 master) 브랜치로 PR을 보낸다.
3. dev 브랜치에서 주요 작업이 merge되면 release(혹은 latest) 브랜치로 merge시키고 실제 서버에 배포한다.
4. 직접 commit은 feat(혹은 fix, hotfix)브랜치에만 한다.
5. dev, master, release 브랜치에는 직접 commit하지 말고 merge만 한다.

## pull request 방법

- fork한 저장소에서 push를 한 후, github repository에서 New pull request를 클릭한다.
- 그러면 코드 비교화면이 나오게 되는데, compare branch, base branch를 확인하여 설정 후, 하단의 코드 변경 내역도 확인 후 이상 없으면 Create pull request 버튼을 클릭한다.
- 변경 내역에 대한 제목과 내용을 작성한 후 Create pull request 버튼을 클릭한다.

# 그 외 알아두면 좋은 키워드들

### 1. rebase : 지난 커밋을 새 커밋처럼 조작할 때 사용

### 2. amend : 깜빡하고 수정 못한 파일이 있을 때, 방금 만든 커밋에 추가하는 경우 사용

### 3. cherry-pick : 커밋 하나만 떼서 지금 브랜치에 붙여 사용할 때 사용

### 4. reset : 과거의 커밋으로 시간을 돌리고 싶을 때 사용

### 5. reverse : 지금 커밋의 변경사항을 되돌리고 싶을 때 사용

### 6. stash : 변경사항을 잠시 킵해두고, 아직 커밋은 하지 않을 경우 사용

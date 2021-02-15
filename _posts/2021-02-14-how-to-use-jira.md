---
title: "[플젝관리] Agile Project 관리 툴, Jira 사용법(draft)"
date: 2021-02-13 23:50
categories: project-management
---

**Jira** is awsome tool for agile project!!

## 개념

- Sprint: 1~2주의 단기 목표를 이루기 위한 시간 단위

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed544d31-a5e3-4877-acc8-6f7c3a4976e0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed544d31-a5e3-4877-acc8-6f7c3a4976e0/Untitled.png)

- **Epic**: 여러개의 요구사항을 포괄할 수 있는 작업 단위
    - 하나의 에픽은 다수의 유저스토리를 포함
- **User Stories**: As a {role}, I want to {action} so that {purpose}
    - 하나의 유저스토리는 다수의 태스크를 포함
- **Task**: 1명의 엔지니어가 최대 2일동안 완료 가능한 개발량.

**추가자료**

- 애자일 방법론으로 프로젝트 진행하기 → [link](https://brunch.co.kr/@svillustrated/27)
- Theme vs Epic vs User Story vs Task → [link](https://www.visual-paradigm.com/scrum/theme-epic-user-story-task/)

---

## Jira Project 만들기

1. [https://www.atlassian.com/software/jira](https://www.atlassian.com/software/jira) 접속
2. 우측 상단의 'Get it free' 버튼 클릭
3. Jira Service Management의 'Select' 버튼 클릭 > Next
4. Google account login
5. Site 이름 입력. (프로젝트 이름)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8b3176f7-3f7a-4632-a614-2d0eed7bd1c2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8b3176f7-3f7a-4632-a614-2d0eed7bd1c2/Untitled.png)

6. 'Agree' 클릭
7. Wait a few minutes...
8. [클래식 템플릿 선택] 페이지에서 칸반 / 스크럽 / 버그 추적 중 하나 선택. 본 문서는 '스크럼' 템플릿을 기준으로 작성됨.

---

## Jira 사용

### 스프린트 보드

좌측 메뉴에서 '활성 스프린트' 선택

- '내 이슈만' 클릭해서 나에게 할당된 태스크만 확인 가능
- 드래그앤 드롭으로 티켓 이동

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/23f7c26e-414b-4463-a5ab-d8bc413c0c65/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/23f7c26e-414b-4463-a5ab-d8bc413c0c65/Untitled.png)

### Jira와 github

- integration: [docs](https://support.atlassian.com/jira-cloud-administration/docs/integrate-with-github/)

- branch 생성
    - 코드과 관련없는 티켓은 생략
    - 각 티켓별 라벨명과 동일하게 브랜치 생성!
    - 예) git branch {branch name}

    ```php
    git branch HANYANG-4
    git chekcout HANYANG-4

    //some changes...
    git status

    git add {filename}
    git commit -m "commit message"
    git push origin HANYANG-4
    ```

    ```php
    //check remote branch list
    git branch -r
    ```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/054a80c7-c629-46a7-b48b-20dd34639730/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/054a80c7-c629-46a7-b48b-20dd34639730/Untitled.png)

---

## Jira 관리

- 탄력성을 위해 Sprint 단위는 1주로! :-)
- 월요일~일요일 단위!

### 1. 한 주의 시작, 스프린트 생성하기

- 좌측 메뉴에서 '백로그' 로 이동
- '스프린트 생성'

### 2. 에픽 생성하기

- '백로그'에서 '에픽' (하단 이미지) 클릭 > '에픽 만들기' 클릭

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ecae2470-e0e1-46a3-bbb0-a7a335d790aa/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ecae2470-e0e1-46a3-bbb0-a7a335d790aa/Untitled.png)

### 3. 태스크 생성하기

**방법1**

- 태스크를 포괄하는 에픽을 선택한 후, '에픽에 이슈 생성' 클릭

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8a27d0c4-e294-4433-9f42-efd5b74b8c00/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8a27d0c4-e294-4433-9f42-efd5b74b8c00/Untitled.png)

- 팝업창에서 아래와 같이 입력
    - 이슈 유형: 작업
    - 요약: 작업을 분명하게 기록
    - 설명: 태스크 설명 입력
    - 담당자: 이름 선택
    - **Sprint: 해당 스프린트 클릭 → 클릭 안하면 스프린트 보드에 안보임!!**

**방법2**

- 좌측 메뉴에서 '활성 스프린트' 클릭 > 상단 '만들기' 버튼 클릭
- 팝업창에서 아래와 같이 입력
    - 이슈 유형: 작업
    - 요약: 작업을 분명하게 기록
    - 설명: 태스크 설명 입력
    - 담당자: 이름 선택
    - **Epic Link: 해당 Epic 선택 필수!**
    - **Sprint: 해당 스프린트 클릭 → 클릭 안하면 스프린트 보드에 안보임!!**
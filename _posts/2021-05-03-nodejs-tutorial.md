---
title: "[nodejs] Nodejs 간단 튜토리얼(1) - 초기셋팅과 패키지 설치"
date: 2021-05-03
categories: nodejs
---

본 포스트에서는 `visual studio code`를 사용합니다.

## 초기 셋팅
#### `nodejs` 다운로드 
- 다운로드 페이지 [http://www.nodejs.org](http://www.nodejs.org)
- 서버로 사용하려면 `LTS`를 다운로드하면 됩니다.
- cmd 창에서 `npm -v` 명령어를 통해 다운로드를 확인. 실패시 환경변수를 확인하거나 다시 다운로드합시다. cmd 창은 `윈도우+R` 누른 뒤 `cmd` 입력 하거나 윈도우 검색에서 `cmd` 입력해서 열 수 있습니다.
#### `package.json` 파일 생성
```
npm init
```
`package.json`은 프로젝트 명세서의 역할. 의존성 모듈 관리도 가능합니다.

#### `node_modules` 디렉토리 생성
```
npm install 
```
개발을 진행하다가 버전 오류나 기타 모듈에 의한 에러가  생겼을 때 `npm install` 명령어로 해결 가능한 경우가 많습니다. 기억해둡시다. `npm i`와 동일합니다.

## 패키지 설치
#### 필요한 패키지 설치 
```
npm install {package_name}
```
##### 여러 개 패키지를 한꺼번에 다운로드할 경우
 패키지명을 연속으로 나열하면 됩니다. `npm install` 대신 `npm i`를 입력해도 무방합니다.
```
npm install {package_name} {package_name} {...}
```

#### 설치 옵션 종류
##### 입력 방법
```
npm install {package_name} {option}
```
`package_name`이 `sample_package`, `option`이 `-g`인 경우에는 아래와 같이 입력하면 됩니다.
```
npm install sample_package -g
```
##### 옵션 종류
##### `-g`
전역(global)으로 설치. `C:\Users\사용자명\AppData\Roaming\npm` 경로에 설치가 됩니다.
##### `--save`
`{package}`를 설치 하면서 package.json파일에 의존성(dependencies) 내용을 추가합니다. 패키지를 설치하기 전후에 dependencies 부분을 확인해보면 이해할 수 있습니다.
##### 그 외
그 외 옵션 종류도 많지만 주로 쓰이는 것은 이 두가지인 것 같습니다. 필요하다면 검색합시다.

#### npm 명령어 옵션
```
npm list            // 현재 경로에 설치된 npm 목록 조회
npm list -g         // Global 경로에 설치된 npm 목록 조회
npm ls              // list의 축약형
npm ls --depth=0    // 현재 경로에 설치된 최상위 모듈만 조회
npm ls -g --depth=0 // Global 경로에 설치된 최상위 모듈만 조회

// 출처: https://kdydesign.github.io/2017/07/15/nodejs-npm-tutorial/ 
```

<br><br>
### 다음 포스트 링크:
[Nodejs 간단 튜토리얼(2) - app.js와 route](https://gyuree-kim.github.io/nodejs/nodejs-tutorial2/)
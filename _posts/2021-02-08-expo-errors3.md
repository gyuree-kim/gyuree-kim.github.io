---
title: "[react-native][expo] (0, _native.createNavigatorFactory) is not a function."
date: 2021-02-08 18:29:28 -0400
categories: react-native
---

## `(0, _native.createNavigatorFactory) is not a function.`
한참을 괴롭힌 에러메세지.. 

* `react-navigation` 재설치
```
npm install @react-navigation/native
```

* `react-native-gesture-handler`, `react-native-reanimated`, `react-native-screens`, `react-native-safe-area-context`, `@react-native-community/masked-view` expo로 재설치
```
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

* `react-native-reanimated`,`react-native-gesture-handler`,`react-native-screens`,`react-native-safe-area-context`,`@react-native-community/masked-view` npm으로 재설치
```
npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

* yarn으로 설치해보기
```
yarn add @react-navigation/native@next
```
버전은 가장 최신버전을 선택했다. 이 방법도 작동하지 않았다.

위의 온갖 명령어로 그냥 재설치, 모듈 삭제하고 재설치 등 구글링해서 찾아낸 방법들로 해결이 되지 않았다ㅠㅠ 

해결방법은 정말 어이없게도....... 에러 메세지에 있었다.
vscode console 의 에러메세지와 태블릿 (모바일 기기에서의 expo)에서의 에러메세지가 달랐는데, 계속 vscode 콘솔만 확인하다가 태블릿을 보니 어처구니없게 <이 중복되었다는 쉬운 에러메세지를 발견하고 바로 해결했다. 왜 두 콘솔이 다른 메세지를 뱉는지는 모르겠지만 쓸데없이 시간을 날렸다ㅠ
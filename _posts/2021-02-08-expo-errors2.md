---
title: "[react-native][expo] Unable to resolve module @react-navigation/native from App.js"
date: 2021-02-08 14:29:28 -0400
categories: react-native
---

## 에러메세지
`App.js`의 `@react-navigation/native`에서 `Unable to resolve module @react-navigation/native`라는 에러가 뜨는 경우가 있다.
```js
import { NavigationContainer } from '@react-navigation/native';
```

## console
콘솔에는 아래와 같은 가이드라인이 나타난다.

```
 1. Clear watchman watches: watchman watch-del-all
 2. Delete node_modules: rm -rf node_modules and run yarn install
 3. Reset Metro's cache: yarn start --reset-cache
 4. Remove the cache: rm -rf /tmp/metro-*
```

## 해결방법1
`react-navigation`을 재설치해준다.
```
npm install react-navigation
npm start -- --reset-cache
```

## 해결방법2
* expo 사용자
expo start -c
```
```

* expo 비사용자
```
npx react-native start --reset-cache
```

* 둘 다 되지 않는다면
```
rm -rf $TMPDIR/metro-bundler-cache-*
```
---
title: "[React Native] How to Create a New RN Project"
date: 2021-01-15 14:29:28 -0400
categories: react-native
---

Welcome to `React Native` world! This post provides simple and fast guide to start the react native proejct. Skip the description and get the codes when you don't have seconds to read the details. :) 

### Install expo
You can choose one of two codes below.
```
npm i -g expo-cli //for npm users
```
```
yarn global add expo-cli //for yarn users
```

### Create a project
```
expo init {projectName} 
```
and select one template when you see the options on screen.

### start the proejct
```
yarn start
```
```
npm start
```
Press `i`(ios) or `a`(android) or `w`(web). 

### Check mobile screen
#### android
If you want to have a look at it through your mobile phone, then just download `expo` app on your phone and read the QR code that you can find on `localhost:19002`. How simple! Of course, you can check the mobile screen on the website with the default emulator.
#### IOS
You have to login in expo With the code below to check the screen on your phone.
```
expo login 
```

### How to check the change on screen
Why is React Native awesome? Because you can check the change as soon as you edit the file. With a few weeks with `android studio`,it was so annoying that I had to wait a few seconds to minutes for a single change. That is one of the reasons why I started to study this lovely react native. 

Go to `App.js` file and edit some text and save. Amazingly, you will see the screen is changed. Test now!

```js
//sample App.js code
import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Text>Hello world</Text>
      <StatusBar style="auto" />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```
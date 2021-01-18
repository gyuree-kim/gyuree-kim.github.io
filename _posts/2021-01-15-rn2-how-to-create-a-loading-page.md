---
title: "[React Native] How to create a loading page with React Native"
date: 2021-01-15 14:29:28 -0400
categories: react-native
---

##App Loading
official docs is here: [click][https://docs.expo.io/versions/latest/sdk/app-loading/]


### Install
```
expo install expo-app-loading
```

### Example
```js
import React from 'react';
import { Image, Text, View } from 'react-native';
import { Asset } from 'expo-asset';
import AppLoading from 'expo-app-loading';

export default class App extends React.Component {
  state = {
    isReady: false,
  };

  render() {
    if (!this.state.isReady) {
      return (
        <AppLoading
          startAsync={this._cacheResourcesAsync}
          onFinish={() => this.setState({ isReady: true })}
          onError={console.warn}
        />
      ); }

    return (
      <View style={{ flex: 1 }}>
        <Image source={require('./assets/snack-icon.png')} />
      </View>
    );
  }

  async _cacheResourcesAsync() {
    const images = [require('./assets/snack-icon.png')];

    const cacheImages = images.map(image => {
      return Asset.fromModule(image).downloadAsync();
    }); 
    return Promise.all(cacheImages);
  }
}
```

### props
#### startAsync (function) 
A `function` that returns a `Promise`, and the `Promise` should resolve when the app is done loading required data and assets.
#### onError (function)
If `startAsync` throws an error, it is caught and passed into the function provided to `onError`.
#### onFinish (function)
 (Required if you provide `startAsync`). Called when `startAsync` resolves or rejects. This should be used to set state and unmount the `AppLoading` component.
#### autoHideSplash (boolean)
Whether to hide the native splash screen as soon as you unmount the AppLoading component. See [SplashScreen module][https://docs.expo.io/versions/v40.0.0/sdk/splash-screen/] for an example.

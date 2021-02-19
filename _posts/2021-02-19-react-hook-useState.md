---
title: "[react] 예제로 이해하는 react hook - 1.useState"
date: 2021-02-19 19:30
categories: react
---

## useState

useState의 개념과 그 편리함을 충분히 이해하기 위해서는 class component의 `state`의 개념을 알고, 사용해본 경험이 있어야 합니다. 만약 그렇지 않을 경우 `state`, `props`, `class component` 등에 대한 간단한 학습 후에 돌아와주세요.

 * Class Component와 state

```js
class Memo extends Component {
    state = {
        text: "",
        //ohter states..
    }

    render(){   
        return(
            <View style={styles.container}>
                <Text style={styles.title}>{props.title}</Text>
            </View>
        );
    }
}
```

위의 코드는 React Native 앱 개발에서 메모 기능을 구현하다가 state를 사용하기 위해 function 에서 class로 바꿨던 코드입니다. 가독성을 위해 불필요한 코드는 삭제했습니다. React hook의 `useState` 기능을 사용하면 굳이 class로 바꿀 필요없이 더 짧은 코드로 작성이 가능합니다. 아래 코드와 비교해보세요!

* function과 useState

```js
function Memo(props) {
    const [text, setText] = useState("");
    //const [stateName, setStateName] = useState("{initial value you want}");
   
    return(
        <View style={styles.container}>
            <Text style={styles.title}>{props.title}</Text>
        </View>
    );
}
```

아직 큰 차이가 안느껴지시죠? `state` 를 컴포넌트 내부에서 값을 변경하고 싶을 떄, useState는 훨씬 편리합니다. `class`의 경우 `setState`를 통해 값을 변경해야하는 것에 반해 `React hook`은 각 변수별로 쉽게 접근하여 값 변경이 가능합니다.

* setState with class

```js
    //declaration
    state = {
        text: "",
        //...other states
    }

    //setup
    setState{
        text: {text},
        //other states
    }
```

* Set state with function

```js
    //declaration
    const [text, setText] = useState("");

    //setup
    setText("changed");
```
---
title: "[react] react form 예제"
date: 2021-02-19 14:04
categories: react
---

https://codesandbox.io/s/react-forms-nbjbk?from-embed=&file=/src/PasswordUpdate.js

아래 코드는 위의 링크 내의 주요 코드입니다.

### src\PasswordUpdate.js
```js
import React, { useState } from "react";

function PasswordUpdate() {
  const [password, setPassword] = useState("");
  const [disabled, setDisabled] = useState(false);

  const handleChange = ({ target: { value } }) => setPassword(value);

  const handleSubmit = async event => {
    setDisabled(true);
    event.preventDefault();
    await new Promise(r => setTimeout(r, 1000));
    if (password.length < 8) {
      alert("8자의 이상의 비밀번호를 사용하셔야 합니다.");
    } else {
      alert(`변경된 패스워드: ${password}`);
    }
    setDisabled(false);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="password"
        name="password"
        value={password}
        onChange={handleChange}
      />
      <button type="submit" disabled={disabled}>
        비밀번호 변경
      </button>
    </form>
  );
}

export default PasswordUpdate;
```

### src\index.js
```js
import React from "react";
import ReactDOM from "react-dom";

import PasswordUpdate from "./PasswordUpdate";

const rootElement = document.getElementById("root");
ReactDOM.render(
  <React.StrictMode>
    <PasswordUpdate />
  </React.StrictMode>,
  rootElement
);
```

---
title: "[nodejs] Nodejs 간단 튜토리얼(3) - model schema 작성"
date: 2021-05-03
categories: nodejs
---

## Get ready
이전 포스트를 학습하고 본 포스트를 읽기를 추천합니다.
- 이전 포스트 링크: [Nodejs 간단 튜토리얼(2)](https://gyuree-kim.github.io/nodejs/nodejs-tutorial2/)

## models/
`user.js` 파일을 생성하고, 아래 코드를 차례로 입력합니다.

#### import
```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
```

#### schema
```js
const user = new Schema({
    _id: Schema.Types.ObjectId,
    email: { type: Number, unique: true },
    name: String,
    createdAt: Date
},{ timestamps: true });
```
- `_id` 새로운 유저가 user schema를 통해 생성될 때 자동으로 부여되는 고유의 값. mysql의 `primary key`와 동일한 개념. 이 외 필드는 필요한대로 작성하면 됨.
- `email` 유저 이메일 필드
- `name` 유저 이름
- `createdAt` 유저가 생성된 시각. 아래 코드처럼 사용 가능.
    ```js
    const User = require('./models/user');
    const newUser = new User({
        createdAt: new Date // 현재 시각
    })
    ```
#### export
```js
module.exports = mongoose.model('user', user);
```

#### full code
```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const user = new Schema({
    _id: Schema.Types.ObjectId,
    email: { type: Number, unique: true },
    name: String,
    password: String,
    createdAt: Date
},
{
    timestamps: true
});

module.exports = mongoose.model('user', user);
```

필요한 모델이 있다면 같은 방식으로 `models/`폴더 내에 js 파일을 생성하여 사용할 수 있습니다. 스키마에 사용 가능한 타입의 종류, 문법, 예시는 [공식문서](https://mongoosejs.com/docs/schematypes.html)에서 확인할 수 있습니다.
<br><br><br>

### 다음 포스트 링크:
[Nodejs 간단 튜토리얼(4) - api 생성](https://gyuree-kim.github.io/nodejs/nodejs-tutorial4/)
# idServer

这次更新剥离了Auth 认证与授权。现在Auth 认证与授权由idServer完成。用户login后，会获得一个token，这个token可以用来访问ticket server 或 arknights server。请将这个token挂在header的Authorization字段下，以便访问ticket server 或 arknights server。

```json
header:
{
    "Authorization": <idServer Token>,
}
```

## login 登录

path: <https://passport.arknights.host/api/v1/login>

method: POST

```json
header:  
{
    "Content-Type": "application/json"
}

body:
{
    "Email": "<string, 用户名>",
    "Password": "<string, 密码>"
}

response:
{
    "code": 1,
    "data": {
        "token": "xxxxx"
    },
    "message": "登录成功"
}
```

```javascript
不想写文档了，自己看代码吧

const axios = require('axios');
let data = JSON.stringify({
  "Email": "xxxx@qq.com",
  "Password": "xxxx"
});

let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://passport.arknights.host/api/v1/login',
  headers: { 
    'Content-Type': 'application/json'
  },
  data : data
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});

```

## register 注册

path: <https://passport.arknights.host/api/v1/register>

method: POST

```json


header:  
{
    "Content-Type": "application/json"
}

body:
{
    "Email": "<string, 用户名>",
    "Password": "<string, 密码>"
}

response:
{
    "code": 1,
    "data": {
        "token": "xxxxx"
    },
    "message": "注册成功"
}
```

```javascript
不想写文档了，自己看代码吧
const axios = require('axios');
let data = JSON.stringify({
  "Email": "563255057111@qq.com",
  "Password": "123456"
});

let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://passport.arknights.host/api/v1/register',
  headers: { 
    'Content-Type': 'application/json'
  },
  data : data
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});

```

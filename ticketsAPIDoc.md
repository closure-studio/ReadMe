# arkTickets

让协助者或管理员更方便查看游戏的各种信息如日志记录等

## API接口

## 发布\创建新的ticket

path: <https://ticket.arknights.host/tickets/>

method: POST

```json
header:  
{
    "Authorization": <idServer Token>,
    "Content-Type": "application/json"
}

body:
{
    "tags": ["<string, 标签>"],
    "attachments": ["<string, 图片链接>"],
    "isPinned": "<boolean, 可选, 是否置顶,管理员权限要求>",
    "author": {
        "nickname": "<string, 游戏内昵称>",
        "avatar": {
            "type": "<string, 头像类型,在arknights相关api可获得>",
            "id": "<string, 头像ID,在arknights相关api可获得>"
        }
    },
    "content": {
        "title": "<string, 大大的标题>",
        "content": "<string, 内容>"
    },
    "isAnonymous": "<boolean, 是否匿名>",
    "gameAccount": "<string, 可选, 游戏账号,格式为G19XXX,在arknights相关api可获得>"
}

body Example:
{
  "tags": ["bug"],
  "attachments": ["https://image-link1.com", "https://image-link2.com"],
  "isPinned": false,
  "author": {
    "nickname": "欧皇大佬",
    "avatar": {
      "type": "DEFAULT",
      "id": "avatar_def_01"
    }
  },
  "content": {
    "title": "根本完全无法使用",
    "content": "请查看附件"
  },
  "isAnonymous": false,
  "gameAccount": "G13xxxxx"
}

```

## 回复某个ticket

path: <https://ticket.arknights.host/tickets/:id/replies>

method: POST

:id为ticket的id

```json

header:  
{
    "Authorization": <idServer Token>,
    "Content-Type": "application/json"
}

body:
{
    "tags": ["<string, 标签>"],
    "attachments": ["<string, 图片链接>"],
    "isPinned": "<boolean, 可选, 是否置顶,管理员权限要求>",
    "author": {
        "nickname": "<string, 游戏内昵称>",
        "avatar": {
            "type": "<string, 头像类型,在arknights相关api可获得>",
            "id": "<string, 头像ID,在arknights相关api可获得>"
        }
    },
    "content": {
        "title": "<string, 大大的标题>",
        "content": "<string, 内容>"
    },
    "isAnonymous": "<boolean, 是否匿名>",
    "gameAccount": "<string, 可选, 游戏账号,格式为G19XXX,在arknights相关api可获得>"
}

```

## 对某个ticket点赞

path: <https://ticket.arknights.host/tickets/:id/vote>

method: POST

:id为ticket的id

```json

header:  
{
    "Authorization": <idServer Token>,
}

body:
{
    
}

```

## 上传附件 (图片)

path: <https://ticket.arknights.host/tickets/attachments>

method: POST

:id为ticket的id

```json

header:  
{
    "Authorization": <idServer Token>,
}

body:
{
    "file": "<上传的图片文件>"
}
```

```javascript

nodeJs axios example:

const axios = require('axios');
const FormData = require('form-data');
const fs = require('fs');

// 创建一个新的FormData实例
let data = new FormData();

// 将文件附加到FormData对象
data.append('file', fs.createReadStream('path/to/your/image.jpg'));

// 配置Axios请求
let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://ticket.arknights.host/tickets/attachments',
  headers: { 
    ...data.getHeaders(),
    "Authorization": "<idServer Token>"
  },
  data : data
};

// 发送请求并处理响应或错误
axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});

```

## 获取tickets列表 不包括replys

path: <https://ticket.arknights.host/tickets>

method: GET

```json

Query Parameters:
- type: 获取tickets的类型，可选值为 "latest" 和 "updated"，默认为 "latest"
- offest: 分页的起始索引，默认为0
- limit: 每页显示的tickets数量，默认为10

header:  
{
    "Authorization": "<idServer Token>"
}

response:
[
    {
        "createdAt": "2023-10-18T08:58:52.002877+08:00",
        "updatedAt": "2023-10-18T08:58:52.002878+08:00",
        "id": "957800f8-207f-47c5-ae97-0801b01669f5",
        "replyTo": null,
        "status": 0, // 0: open, 1: closed
        "tags": ["bug"],
        "attachments": [
            "https://image-link1.com/1.jpg",
            "https://image-link2.com/2.jpg"
        ],
        "isHidden": false,
        "isPinned": false,
        "authorUUID": "", // only display for admin
        "author": {
            "uuid": "", // only display for admin
            "title": "", 
            "nickname": "", 
            "avatar": {
                "type": "",
                "id": ""
            }
        },
        "content": {
            "id": "",
            "title": "",
            "content": ""
        },
        "votes": 0,
        "isAnonymous": false,
        "gameAccount": "" // only display for admin
    }
]


```


## 获取tickets列表 不包括replys和content

path: <https://ticket.arknights.host/tickets>

method: GET

```json

Query Parameters:
- type: 获取tickets的类型，可选值为 "latest" 和 "updated"，默认为 "latest"
- offest: 分页的起始索引，默认为0
- limit: 每页显示的tickets数量，默认为10

header:  
{
    "Authorization": "<idServer Token>"
}

response:
[
    {
        "createdAt": "2023-10-18T08:58:52.002877+08:00",
        "updatedAt": "2023-10-18T08:58:52.002878+08:00",
        "id": "957800f8-207f-47c5-ae97-0801b01669f5",
        "replyTo": null,
        "status": 0, // 0: open, 1: closed
        "tags": ["bug"],
        "attachments": [
            "https://image-link1.com/1.jpg",
            "https://image-link2.com/2.jpg"
        ],
        "isHidden": false,
        "isPinned": false,
        "authorUUID": "", // only display for admin
        "author": {
            "uuid": "", // only display for admin
            "title": "", 
            "nickname": "", 
            "avatar": {
                "type": "",
                "id": ""
            }
        },
        "content": {
            "id": "",
            "title": "",
            "content": ""
        },
        "votes": 0,
        "isAnonymous": false,
        "gameAccount": "" // only display for admin
    }
]


```

## 获取ticket 不包括replys

path: <https://ticket.arknights.host/tickets/:id>

method: GET

:id 为ticket的id

```json


header:  
{
    "Authorization": "<idServer Token>"
}

response:
{
    "createdAt": "2023-10-18T08:58:52.002877+08:00",
    "updatedAt": "2023-10-18T08:58:52.002878+08:00",
    "id": "957800f8-207f-47c5-ae97-0801b01669f5",
    "replyTo": null,
    "status": 0,
    "tags": ["bug"],
    "attachments": [
        "https://image-link1.com",
        "https://image-link2.com"
    ],
    "isHidden": false,
    "isPinned": false,
    "authorUUID": "",  // only display for admin
    "author": {
        "uuid": "", // only display for admin
        "title": "",
        "nickname": "欧皇大佬",
        "avatar": {
            "type": "DEFAULT",
            "id": "avatar_def_01"
        }
    },
    "content": {
        "id": "957800f8-207f-47c5-ae97-0801b01669f5",
        "title": "根本完全无法使用",
        "content": "请查看附件"
    },
    "votes": 1,
    "isAnonymous": false,
    "gameAccount": "" // only display for admin
}

```

## 获取ticket的回复

path: <https://ticket.arknights.host/tickets/:id/replies>

method: GET

:id 为ticket的id

```json


header:  
{
    "Authorization": "<idServer Token>"
}

response:
[
    {
        "createdAt": "2023-10-18T09:12:32.360671+08:00",
        "updatedAt": "2023-10-18T09:12:32.360671+08:00",
        "id": "c104321c-4acb-4d50-a217-72c1bac98b84",
        "replyTo": "957800f8-207f-47c5-ae97-0801b01669f5",
        "status": 0,
        "tags": ["bug"],
        "attachments": [
            "https://image-link1.com",
            "https://image-link2.com"
        ],
        "isHidden": false,
        "isPinned": false,
        "authorUUID": "", // only display for admin
        "author": {
            "uuid": "", // only display for admin
            "title": "",
            "nickname": "欧皇大佬",
            "avatar": {
                "type": "DEFAULT",
                "id": "avatar_def_01"
            }
        },
        "content": {
            "id": "c104321c-4acb-4d50-a217-72c1bac98b84",
            "title": "根本完全无法使用",
            "content": "请查看附件"
        },
        "votes": 0,
        "isAnonymous": false,
        "gameAccount": "" // only display for admin
    },
    // ...更多回复数据
]

```


## 更新\修改 ticket或者reply

path: <https://ticket.arknights.host/tickets/:id>

method: Put

:id 为ticket的id

```json

header:  
{
    "Authorization": "<idServer Token>"
}

body:
{
    "status": "<int, 可选, 0: open, 1: closed>",
    "tags": ["<string, 标签>"],
    "attachments": ["<string, 图片链接>"],
    "isHidden": "<boolean, 可选, 隐藏>",
    "isPinned": "<boolean, 可选, 是否置顶,管理员权限要求>",
    "content": {
        "id": "<string, 该ticket,reply的id>",
        "title": "<string, 大大的标题>",
        "content": "<string, 内容>"
    },
    "isAnonymous": "<boolean, 是否匿名>",
}


body example:
{
    "status": 1, // 完成，解决
    "tags": ["bug", "ui"],
    "attachments": [
        "https://image-link1.com",
        "https://image-link2.com"
    ],
    "isHidden": false,
    "isPinned": false, // admin 权限
    "content": {
        "id": "c104321c-4acb-4d50-a217-72c1bac98b84",
        "title": "更新标题",
        "content": "更新内容"
    },
    "isAnonymous": false
}
```

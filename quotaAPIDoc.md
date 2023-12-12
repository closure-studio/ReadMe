# arkQuota

arkQuota 将负责管理游戏的创建，删除与修改密码

通过查询  https://registry.closure.setonink.com/api/users/me 获取该账号的资源(slot，也就是槽位)

每个槽位都具有自己的UUID，游戏的创建，删除与修改密码将通过这个UUID进行操作

一般情况下，一个用户将会有3个槽位，也就是说能创建三个游戏。

第一个槽位是
            "ruleFlags": [
                "slot_account_format_is_phone",
                "slot_account_sms_verified"
            ],

只有完成了第一个游戏账号的短信验证才允许添加第二个第三个游戏。

请注意 对于 游戏的创建 删除 和修改密码， 都是使用同一个 api https://registry.closure.setonink.com/api/slots/gameAccount 和同一个 method Post



获取当前用户的槽位
```
import axios from 'axios';

const options = {
  method: 'GET',
  url: 'https://registry.closure.setonink.com/api/users/me',
  headers: {
    Authorization: 'Bearer xxxxx',
    Accept: 'application/json'
  }
};

try {
  const { data } = await axios.request(options);
  console.log(data);
} catch (error) {
  console.error(error);
}
```

创建游戏
```
import axios from 'axios';

const options = {
  method: 'POST',
  url: 'https://registry.closure.setonink.com/api/slots/gameAccount', 
  params: {uuid: '18be284a-dccc-43bd-a55a-072d92940913'},// 这里是指slot的UUID
  headers: {
    Authorization: 'Bearer xxxxx',
    'Content-Type': 'application/json',
    Accept: 'application/json',
    'x-platform': 'website' // 注意 可以填写website或者app,
    'token':<geetest 或者 google的recaptcha>,
  },
  data: {account: '139xxxxxx', platform: '1', password: 'xxxxx'} // platform == 1 表示官服， platform == 2 表示 B服
};

try {
  const { data } = await axios.request(options);
  console.log(data);
} catch (error) {
  console.error(error);
}
```


删除游戏
```
import axios from 'axios';

const options = {
  method: 'POST',
  url: 'https://registry.closure.setonink.com/api/slots/gameAccount', 
  params: {uuid: '18be284a-dccc-43bd-a55a-072d92940913'},// 这里是指slot的UUID
  headers: {
    Authorization: 'Bearer xxxxx',
    'Content-Type': 'application/json',
    'Accept': 'application/json'，
    'x-platform': 'website' // 注意 可以填写website或者app,
    'token':<geetest 或者 google的recaptcha>,
  },
  data: {account: null} // account 设置为null表示删除
};

try {
  const { data } = await axios.request(options);
  console.log(data);
} catch (error) {
  console.error(error);
}
```

修改游戏密码
```
import axios from 'axios';

const options = {
  method: 'POST',
  url: 'https://registry.closure.setonink.com/api/slots/gameAccount', 
  params: {uuid: '18be284a-dccc-43bd-a55a-072d92940913'},// 这里是指slot的UUID
  headers: {
    Authorization: 'Bearer xxxxx',
    'Content-Type': 'application/json',
    'Accept': 'application/json'，
    'x-platform': 'website' // 注意 可以填写website或者app,
    'token':<geetest 或者 google的recaptcha>,
  },
  data: {account: "139xxxxxx",password: 'xxxxx新的密码'}
};

try {
  const { data } = await axios.request(options);
  console.log(data);
} catch (error) {
  console.error(error);
}
```
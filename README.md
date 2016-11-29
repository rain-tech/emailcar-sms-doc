# Emailcar 短信API接口


## 短信模板获取


`GET/POST` `http://test.emailcar.net/sms/tpl_get`

### Request

| 参数名 | 是否必填 | 说明 |
|-------|---------|-----|
| api_user | 必填 | 用户名 |
| api_pwd | 必填 | 用户原密码 | 
| id      | 非必填 | 模板 id |

### Response
**查询所有短信模板信息**

```html
GET http://test.emailcar.net/sms/tpl_get?api_user=xxxxx&api_pwd=xxxxx  

POST http://test.emailcar.net/sms/tpl_get
api_user=xxxxx
api_pwd=xxxxx
```
```js
{
    "msg": "获取成功",
    "status": "success",
    "data": {
        "lists": [
            {
                "id": "2123",
                "title": "双11推广提醒",
                "amount": 1,
                "content": "【emailcar】双11马上就要开始了，开始使用emailcar向您的会员发送推广邮件吧！",
                "status": "正在审核中"
            },
            {
                "id": "4212",
                "title": "注册成功提示",
                "amount": 1,
                "content": "【emailcar】恭喜您注册成功",
                "status": "正在审核中"
            }
        ]
    }
}
```

| 属性 | 详细说明| 示例 |
|-----|--------|
| `msg` | 查询消息 | `"获取成功"` `"用户密码错误"` `"模板不存在"` |
| `status` | 查询结果 | `"success"` `"error"` |
| `data` | 详细信息 | |
| `data.lists` | 短信模板数据 | |
| `data.lists[].id` | 模板ID |
| `data.lists[].title` | 短信模板标题 | |
| `data.lsits[].amount` | 短信条数 | |
| `data.lists[].content` | 短信模板内容 | |
| `data.lists[].status` | 审核状态 | `"正在审核中"` `"审核通过"` `"未通过审核，原因：包含非法内容"` |


### 查询单个短信模板的信息

```html
GET http://test.emailcar.net/sms/tpl_get?api_user=xxxxx&api_pwd=xxxxx&id=2312  

POST http://test.emailcar.net/sms/tpl_get
api_user=xxxxx
api_pwd=xxxxx
id=2312
```
```js
{
    "msg": "获取成功",
    "status": "success",
    "data": {
        "lists": [
            {
                "id": "2123",
                "title": "双11推广提醒",
                "amount": 1,
                "content": "【emailcar】双11马上就要开始了，开始使用emailcar向您的会员发送推广邮件吧！",
                "status": "正在审核中"
            }
        ]
    }
}
```

## 短信模板添加


`GET/POST` `http://test.emailcar.net/sms/tpl_add`

### Request

| 参数名 | 是否必填 | 说明 |
|-------|---------|-----|
| api_user | 必填 | 用户名 |
| api_pwd | 必填 | 用户原密码 | 
| content| 必填 | 短信模板内容，**不包括签名** |
| sign | 必填 | 短信签名 3~8个字 |
| title | 必填 | 短信模板标题 |

```html
GET http://test.emailcar.net/sms/tpl_add?api_user=xxxxx&api_pwd=xxxxx&content=双11马上就要开始了，开始使用emailcar向您的会员发送推广邮件吧！&sign=emailcar&title=双11推广提醒

POST http://test.emailcar.net/sms/tpl_add
api_user=xxxxx
api_pwd=xxxxx
content=双11马上就要开始了，开始使用emailcar向您的会员发送推广邮件吧！
sign=emailcar
title=双11推广提醒
```

```js
{
    "msg": "获取成功",
    "status": "success",
    "data": {
        "id": "1323"
    }
}
```

**短信发送计费说明：** 短信+签名包括2个 括号在内，总字数在**70字以内的每个手机计费1条**，**超过70个字按照67个字一条计费**

## 短信发送

`GET/POST` `http://test.emailcar.net/sms/send`

```html
GET http://test.emailcar.net/sms/send?api_user=xxxxx&api_pwd=xxxxx&template_id=1323&mobiles=13612340000,13612349999

POST http://test.emailcar.net/sms/send
api_user=xxxxx
api_pwd=xxxxx
template_id=1323
mobiles=13612340000,13612349999
```
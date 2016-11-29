# Emailcar 短信API接口


## 短信模板获取


`GET` `http://test.emailcar.net/sms/tpl_get`

### Request

| 参数名 | 是否必填 | 说明 |
|-------|---------|-----|
| api_user | 必填 | 用户名 |
| api_pwd | 必填 | 用户原密码 | 
| id      | 非必填 | 模板 id |

### Response

```js
{
    "msg": "获取成功",
    "status": "success",
    "data": {
        "lists": [
            {
                "title": "test",
                "amount": 1,
                "content": "短信模板内容",
                "status": "正在审核中"
            }
        ]
    }
}
```

| 属性 | 详细说明| 示例 |
|-----|--------|
| `msg` | 查询消息 | `"获取成功"` `"用户密码错误"` `"模板不存在"` |
| `status` | 查询结果 | `success` `error` |
| `data` | 详细信息 | |
| `data.lists` | 短信模板数据 | |
| `data.lists[].title` | 短信模板标题 | |
| `data.lsits[].amount` | 短信条数 | |
| `data.lists[].content` | 短信模板内容 | |
| `data.lists[].status` | 审核状态 | `"正在审核中"` `"审核通过"` `"未通过审核"，原因：包含非法内容"` |
# 邮件接口须知

---

**支持版本**

无论是免费版还是付费版，EmailCar都支持用户使用邮件API接口。区别在于付费版用户使用的投递通道是独立并且进行过相关备案的，投递效果更好，并发能力更强。免费版用户使用共享通道根据历史投递情况分配不同信誉的共享通道，在投递时效和并发能力上相对付费版来说要差一些，但是基本使用是一样的。



**申请接口**

注册并激活EmailCar后，进入系统在左侧导航栏中选择“邮件营销”-“触发邮件”，点击页面中的“申请您的邮件API接口”按钮提交后台审核。审核通过后，就可以开始通过接口进行调用发送邮件。

![](/assets/QQ截图20170301105923.jpg)

**注意事项**

1. API接口调用的用户名密码就是登陆EmailCar的用户名密码；

2. 接口一次性提交邮件最多50封；

3. 接口每秒钟只能提交1次；

4. 接口支持WebAPI和SMTP两种方式调用，建议用WebAPI，SMTP获取的统计数据比较单一；

5. 接口提交的邮件内容只允许是事务性内容，对于营销类型内容系统会根据收件人阅读情况进行选择性给予开放；

6. 如果遇到用户被大量收件人具备为垃圾邮件，系统将自动关闭账户使用权限。



# 触发发送

---

**URL**

| [http://webapi.emailcar.net/v1/mail/send](http://webapi.emailcar.net/v1/mail/send) |
| :--- |


**HTTP请求方式**

| post |
| :--- |


**参数说明**

| 参数 | 类型 | 必须 | 说明 |
| :--- | :--- | :--- | :--- |
| api\_user | string | 是 | EmailCar的登录帐号\(用户名和邮箱都可以\) |
| api\_pwd | string | 是 | EmailCar的登录密码，可以使用原字符串密码或32位MD5加密 |
| from | string | 是 | 发件人邮箱地址，必须在EmailCar事务性发件人平台中 如 support@emailcar.net |
| fromname | string | 是 | 发件人名称. 显示如: 客服服务 \(support@emailcar.net\) |
| to | string | 是 | 收件人地址. 多个地址使用';'分隔, 如 test@emailcar.net;test1@emailcar.net 注意单个收件人地址是一个任务ID，多个地址返回多个任务ID。并且只支持最多50个收件人地址 |
| subject | string | 是 | 邮件发送标题. 不能为空 |
| html | string | 是 | 邮件的内容. 不能为空, 可以是文本格式或者 HTML 格式 |
| replyto | string | 否 | 默认的回复邮件地址. 如果 replyto 没有或者为空, 则默认的回复邮件地址为 from |
| open | int | 否 | 是否打开开信统计，如果打开开信统计会获取模板插入开信统计代码，默认关闭 |
| click | int | 否 | 是否打开链接点击统计，如果打开链接点击统计会获取模板把所有链接替换成可以统计的代码，默认关闭 |

**返回 JSON格式**

| {"status":"error","msg":"失败原因"} | 发送失败 |
| :--- | :--- |
| {"data":{"return":{"id":id}},"msg":"邮件发送成功","status":"success"} | 发送成功 |

**请求，返回值示例**

```
curl -d 'api_user=***&api_pwd=***&from=support@emailcar.net&fromname=Emailcar&to=test@emailcar.net;test1@emailcar.net&subject=欢迎使用Emailcar&html=Emailcar为您服务！' http://webapi.emailcar.net/v1/mail/send
# 失败返回值 ( 如用户名或密码错误 )
{
    "msg": "用户名或密码错误",
    "status": "error"
}
#成功返回值
{
    "data": {
        "return": {
            "id": 873216184254383362
        }
    },
    "msg": "邮件发送成功",
    "status": "success"
}

```



# 数据统计概况

---

# **URL**

| [http://webapi.emailcar.net/v1/mail/stats](http://webapi.emailcar.net/v1/mail/stats) |
| :--- |


**HTTP请求方式**

| post |
| :--- |


参数说明

| 参数 | 类型 | 必须 | 说明 |
| :--- | :--- | :--- | :--- |
| api\_user | string | 是 | EmailCar的登录帐号\(用户名和邮箱都可以\) |
| api\_pwd | string | 是 | EmailCar的登录密码，可以使用原字符串密码或32位MD5加密 |
| start\_date | string | 否 | 开始日期, 格式为1984-01-02 |
| end\_date | string | 否 | 结束日期, 格式同开始日期 |
| send\_id | int | 否 | 发送的时候返回的ID，当有这个参数的时候日期无效 |

**返回 JSON格式**  


| {"status":"error","msg":"失败原因"} | 获取失败 |
| :--- | :--- |
| {"data":{"total":{"failure":失败总数,"request":请求总数,"success":成功总数}},"msg":"数据成功获取","status":"success"} | 获取成功 |

**请求，返回值示例**

```
curl -d 'api_user=***&api_pwd=***&start_date=2012-01-01&end_date=2012-01-02' http://webapi.emailcar.net/v1/mail/stats
#失败返回值 (如用户名或密码错误) 
{
    "msg": "用户名或密码错误",
    "status": "error"
}
#成功返回值 
{
    "data": {
        "total": {
            "failure": 84,
            "request": 436,
            "success": 352
        }
    },
    "msg": "数据成功获取",
    "status": "success"
}

```

**成功返回值说明**

| 参数 | 返回值 | 说明 |
| :--- | :--- | :--- |
| failure  | 84  | 失败总数 |
| request  | 436  | 请求总数 |
| success  | 352  | 成功投递总数  |



# 发送数据详情

---

**URL**

| [http://webapi.emailcar.net/v1/mail/lists](http://webapi.emailcar.net/v1/mail/lists) |
| :--- |


**HTTP请求方式**

| post |
| :--- |


参数说明

| 参数 | 类型 | 必须 | 说明 |
| :--- | :--- | :--- | :--- |
| api\_user | string | 是 | EmailCar的登录帐号\(用户名和邮箱都可以\) |
| api\_pwd | string | 是 | EmailCar的登录密码，可以使用原字符串密码或32位MD5加密 |
| start\_date | string | 否 | 开始日期, 格式为1984-01-02 |
| end\_date | string | 否 | 结束日期, 格式同开始日期 |
| send\_id | int | 否 | 发送的时候返回的ID，当有这个参数的时候日期无效 |
| p | int | 否 | 页数，默认第一页 |
| page\_size | int | 否 | 每页显示条数，默认20条，不能超过40条 |

**返回 JSON格式**

| {"status":"error","msg":"失败原因"} | 获取失败 |
| :--- | :--- |
| {"data":{"0":{"failure\_msg":"","from":"",..},...."msg":"数据成功获取","status":"success"} | 获取成功 |

**请求，返回值示例**

```
curl -d 'api_user=***&api_pwd=***&start_date=2012-01-01&end_date=2012-01-02' http://webapi.emailcar.net/v1/mail/lists
# 失败返回值 ( 如数据为空错误 )
{
    "msg": "没有找到数据",
    "status": "error"
}
# 成功返回值
{
    "data": {
        "0": {
            "failure_msg": "",
            "from": "mail@service.emailcar.net",
            "fromname": "EmailCar会员中心",
            "guest_agent": "",
            "guest_count": "0",
            "guest_ip": "",
            "replyto": "mail@service.emailcar.net",
            "send_time": "2015-11-17 15:29:51",
            "status": "2",
            "subject": "yjy111用户提交了一个任务((AD)【活动最后三天！】反法西斯纪念币免费领【包邮】知识啤酒无限畅饮！！)待审核"
            },
            "return": {
              "id": 873216184254383362
            }
        },
        "1": {
            "failure_msg": "",
            "from": "mail@service.emailcar.net",
            "fromname": "EmailCar会员中心",
            "guest_agent": "",
            "guest_count": "0",
            "guest_ip": "",
            "replyto": "mail@service.emailcar.net",
            "send_time": "2015-11-17 15:45:13",
            "status": "2",
            "subject": "yangpudianzi用户提交了一个任务(广州阳普邀您参加11月27日物联网最新技术体验及红酒品酒会)待审核"
        }
    },
    "msg": "邮件发送成功",
    "status": "success"
}
```

**成功返回值说明**

| 返回参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| subject | string | 邮件发送的标题 |
| from | string | 发件人邮址 |
| replyto | string | 回复邮址 |
| fromname | string | 发件人名称 |
| send\_time | string | 邮件发送时间 Y-m-d H:i:s |
| status | string | 邮件发送状态 1 发送中 2 发送成功 3 发送失败 |
| guest\_count | string | 邮件打开次数 |
| guest\_ip | string | 邮件打开IP |
| guest\_agent | string | 邮件具体发送信息 |
| failure\_msg | string | 邮件发送错误的原因 |



# 查询余额

---

**URL**

| [http://webapi.emailcar.net/v1/user/balance](http://webapi.emailcar.net/v1/user/balance) |
| :--- |


**HTTP请求方式**

| post |
| :--- |


参数说明

| 参数 | 类型 | 必须 | 说明 |
| :--- | :--- | :--- | :--- |
| api\_user | string | 是 | EmailCar的登录帐号\(用户名和邮箱都可以\) |
| api\_pwd | string | 是 | EmailCar的登录密码，可以使用原字符串密码或32位MD5加密 |

**返回 JSON格式**

| {"status":"error","msg":"失败原因"} | 获取失败 |
| :--- | :--- |
| {"data":{"points":{"ad":"剩余广告点数","email":"剩余邮箱点数","sms":"剩余短信点数"}},"msg":"数据成功获取","status":"success"} | 获取成功  |




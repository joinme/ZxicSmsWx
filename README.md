# 基于中兴方案的 LTE USB Dongle 的短信转发器

## 配置文件

配置文件以 JSON 为主要结构，下面是一个配置文件的范本。

```
{
    "wx_key": "d223596f-326d-4778-b7ff-888888888",,
    "modems": [
        {
            "type": "zxic_web_new",
            "name": "13800138000",
            "modem_ip": "172.17.0.1",
            "login_password": "admin"
        },
        {
            "type": "zxic_web_old",
            "name": "15666666666",
            "modem_ip": "172.17.0.5",
            "login_password": "admin"
        }
    ]
}

```

### 配置文件键说明

|         键         |                            说明                                                                 |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| wx_key      |  微信机器人的key。例如 https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key={wx_key}                                           |

### Modems 配置键说明

|         键         |                            说明                                                                 |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| type               | 设备固件类型，只能是 `zxic_web_new` 或者 `zxic_web_old` 两个值。                                   |
| name               | 设备名称，用于在发送短信是区别不同的设备。                                                          |
| modem_ip           | Modem 的后台管理界面的 IP。                                                                       |
| login_password     | 登录 Modem 后台管理界面的密码。                                                                   |

Modem 的 type 用于区分不同的固件类型，因为两种固件访问的 URL 不一样。
需要确认设备是哪种固件类型只需要用浏览器打开后台管理界面，
然后按下 F12 按钮打开开发者工具，点到网络 Tab，
查看刷出来的内容是 `proc_get` 还是 `goform_get_cmd_process`。
如果是 `proc_get`，type 则填写 `zxic_web_new`，
如果是 `goform_get_cmd_process`，type 则填写 `zxic_web_old`。


## 机器人命令参考

`/get_devices`：无参数，用于获取设备列表及设备状态。

`/send_sms`：发送短信，格式为 `/send_sms[空格]设备名称[空格]接收人手机号[空格]短信内容`，例如 `/send_sms 13800138000 10086 cxll` 是使用名为“13800138000”的设备向“10086”发送“cxll”
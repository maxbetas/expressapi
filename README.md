**目前只支持极兔 用的人多的话 增加其他快递 目前顺丰 中通 圆通 申通的SDK正在封装中 都是调用的官方接口 目前没有限制频率**


### 批量运单查询

**请求地址**：`https://shop_api.uinstall.eu.org:888/shop/api/v1/express/track`

**请求方法**：`POST`

**请求参数**：
```json
{
    "orders": [
        {
            "number": "JT1234567890",
            "company": "jt"
        },
        {
            "number": "JT0987654321",
            "company": "jt"
        },
        {
            "number": "SF1234567890",
            "company": "sf"
        }
    ]
}
```

**参数说明**：
- `orders`: 订单列表
  - `number`: 运单号
  - `company`: 快递公司代码
- 每个快递公司的单次查询限制：
  - 极兔快递（jt）：最多 50 个运单
  - 顺丰速运（sf）：最多 20 个运单
  - 中通快递（zto）：最多 30 个运单
- 可以在一次请求中查询不同快递公司的运单
- 超过限制会返回错误

**响应示例**：
```json
{
    "code": 0,
    "message": "success",
    "data": {
        "JT1234567890": {
            "success": true,
            "info": {
                "number": "JT1234567890",
                "company": "极兔快递",
                "status": "运输中",
                "update_time": "2024-01-15 10:30:00",
                "traces": [
                    {
                        "time": "2024-01-15 10:30:00",
                        "status": "运输中",
                        "location": "广东省深圳市宝安区",
                        "message": "快件已从深圳宝安分拨中心发出"
                    }
                ]
            }
        },
        "JT0987654321": {
            "success": false,
            "error": "未找到物流信息"
        },
        "SF1234567890": {
            "success": true,
            "info": {
                "number": "SF1234567890",
                "company": "顺丰速运",
                "status": "已签收",
                "update_time": "2024-01-15 09:00:00",
                "traces": [
                    {
                        "time": "2024-01-15 09:00:00",
                        "status": "已签收",
                        "location": "北京市朝阳区",
                        "message": "已签收，签收人：张先生"
                    }
                ]
            }
        }
    }
}
```

# 公共参数及加密秘钥生成


### 接口文档公共规则:

> HTTP 请求方法:POST,GET

> 注意: 仅当请求类型为POST且Content-type为application/x-www-form-urlencoded将从POST解析参数,其他情况从GET解析参数

####  API域名:

> https://rest-api.huishoubao.com/

####  公共请求参数说明:

| 参数 | 类型 | 是否必填 | 最大长度 | 描述 | 示例值 |
| ---- | ---- | -------- | -------- | ---- | ------ |
|app_key		|String	|是		|32		|应用ID			|caigouxia |
|method		|String	|是		|128		|接口名称			|product.detail|
|version		|String	|是		|3		|调用的接口版本		|固定为：1.0|
|timestamp	|String	|是		|19		|发送请求的时间<br/>格式"yyyy-MM-dd HH:mm:ss"	| 	2014-07-24 03:07:50|
|sign		|String	|是		|344		|请求参数的签名串	|生成方式参见`签名生成`|
|content	|String	|是		|无		|请求参数的集合,JSON字符串.<br/>除公共参数外所有请求参数都必须放在这个参数中传递|无|
|token	|String	|否		|无		|指定用户授权TOKEN,非必填|MTQtSldES1RIUVVZT0NGUkVNUEdJQlpBTlhM|

##### 请求参数示例:

> app_key=caigouxia&content=%7B%22detail_id%22%3A%22111%22%2C%22attr%22%3A%22Brand%2CSale%22%7D&method=product.detail&timestamp=2022-05-16+14%3A13%3A07&version=1.0&sign=0cf4ecb5c5edef7215a6e3e43a9e0c9b


#### 公共返回参数说明:

| 参数 | 类型 | 是否必返回 | 最大长度 | 描述 |
| ---- | ---- | ---------- | -------- | ---- |
|request_id					|String	|是			|32		|每次请求访问的唯一标识|
|result_response.code		|String	|是			|12		|系统状态码,除200外均为异|常
|result_response.sub_code	|String	|是			|12		|业务状态标识,一般为 |success,可能为空其他
|result_response.message		|String	|是			|64		|相关消息|
|response					|Array	|是			|不限		|业务相关数据,可能为:{}|

##### 返回参数示例:

```
{
	"request_id":"ddf817fd3def5c7",
    "result_response": {
        "code": "200",
        "sub_code": "product_not_find",
        "message": "商品未找到"
    },
    "response": {
		"product":{
			"name":"Iphone",
			"sku":"111_11"
		}
	}
}
```


####  签名生成方法及示例:

> `注意`: 仅使用 timestamp,app_key,method,version,content,token 这些参数参与秘钥生成,其中token为可选值

##### 示例秘钥

>  c97b1d22270ddf817fd3def5c7a4e712

##### 请求参数示例:

> timestamp=2022-05-13+16%3A54%3A51&app_key=caigouxia&method=product.detail&version=1.0&content=%7B%22detail_id%22%3A%22111%22%2C%22attr%22%3A%22Brand%2CSale%22%7D

##### 按KEY排序:

> app_key=caigouxia&content=%7B%22detail_id%22%3A%22111%22%2C%22attr%22%3A%22Brand%2CSale%22%7D&method=product.detail&timestamp=2022-05-13+16%3A54%3A51&version=1.0

##### 签名生成:[连接KEY后MD5]

>  app_key=caigouxia&content=%7B%22detail_id%22%3A%22111%22%2C%22attr%22%3A%22Brand%2CSale%22%7D&method=product.detail&timestamp=2022-05-16+14%3A13%3A07&version=1.097b1d22270ddf817fd3def5c7a4e712


#####  MD5后生成结果[转为小写]:

>  0cf4ecb5c5edef7215a6e3e43a9e0c9b

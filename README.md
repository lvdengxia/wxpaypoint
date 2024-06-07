# wechatpaypoint

微信支付分


## Requirement


1. PHP >= 7.2
2. **[Composer](https://getcomposer.org/)**
3. openssl 拓展


## Installation

```shell
$ composer require lvdengxia/wxpaypoint 1.0.0
```

## Usage

先初始化:

```php
<?php

use WxPayPoint\Request\WxPayPointRequest;

$config = [
	'appid'       => 'wx0f3a4b0dcafxxxx',//公众账号ID
	'mch_id'      => '157952xxxx',   //支付商户号
	'service_id'  => '0000200000000015984369xxxxxxxxxx',//服务ID
	'key'         => '009DA8BDD9A5AECFED72F3xxxxxxxxxx',//支付key
	'v3key'       => '1BA00441A40AD15D1D2C54xxxxxxxxxx',//支付v3key
	'serial_no'   => '3F389F9F2D414CF22E156Dxxxxxxxxxxxxxxxxx',//证书序号
	'private_key' => '/cert/apiclient_key.pem',//证书
	'public_key'  => '/cert/apiclient_cert.pem',
];

$app = WxPayPointRequest::getInstance( $config );

```

使用公共API中<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter3_1.shtml">创建支付分订单</a>API:

```php
$param = [
	'out_order_no'         => '202009171036252525',
	'service_introduction' => '租借飞机服务',
	'time_range'           => [
		'start_time' => date( "YmdHis" )
	],
	'risk_fund'            => [
		'name'   => 'DEPOSIT',
		'amount' => 9900,
	],
	'notify_url'           => 'https://www.baidu.com'
];
$data = $app->createBill($param);
```
以上为基本使用的操作，API需要的参数请根据 [微信支付分通用文档](https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter1_1.shtml)需求进行传入，无需考虑参数是path、query、body，把必选的参数传入即可。

## More

免确认模式特有API中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter3_9.shtml">创单结单合并API</a>:

```php
$data = $app->directComplete($param);
```

免确认特有API（普通授权方式）中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter3_8.shtml">查询用户授权状态API</a>:

```php
$data = $app->userServiceState($param);
```

免确认特有API（普通授权方式）中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter9_1.shtml">商户解除用户授权关系API</a>:

```php
$data = $app->terminate($param);
```

免确认特有API（预授权方式）中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter5_1.shtml">商户预授权API</a>:

```php
$data = $app->permission($param);
```

免确认特有API（预授权方式）中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter5_2.shtml">查询与用户授权记录（授权协议号）API</a>:

```php
$data = $app->authCodePermission($param);
```

免确认特有API（预授权方式）中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter5_3.shtml">解除用户授权关系（授权协议号）API</a>:

```php
$data = $app->authCodeTerminate($param);
```

免确认特有API（预授权方式）中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter5_4.shtml">查询与用户授权记录（openid）API</a>:

```php
$data = $app->openidPermission($param);
```

免确认特有API（预授权方式）中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter5_5.shtml">解除用户授权关系（openid）API</a>:

```php
$data = $app->openidTerminate($param);
```

公共API中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter3_1.shtml">创建支付分订单API</a>:

```php
$data = $app->createBill($param);
```

公共API中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter3_2.shtml">查询支付分订单API</a>:

```php
$data = $app->queryBill($param);
```

公共API中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter3_3.shtml">取消支付分订单API</a>:

```php
$data = $app->cancelBill($param);
```

公共API中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter3_4.shtml">修改订单金额API</a>:

```php
$data = $app->modifyBill($param);
```

公共API中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter3_5.shtml">完结支付分订单API</a>:

```php
$data = $app->completeBill($param);
```

公共API中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter3_6.shtml">商户发起催收扣款API</a>:

```php
$data = $app->payBill($param);
```

公共API中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter3_7.shtml">同步服务订单信息API</a>:

```php
$data = $app->syncBill($param);
```

公共API中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/apis/chapter6_1_26.shtml">申请退款API</a>:

```php
$data = $app->applyRefund($param);
```

公共API中的<a href="https://pay.weixin.qq.com/wiki/doc/apiv3/apis/chapter6_1_27.shtml">查询单笔退款API</a>:

```php
$data = $app->queryRefund($param);
```

异步通知操作静态类

异步通知参数解密:

```php
$data=NotifyHandle::notifyDecode($associatedData, $nonce, $ciphertext, $v3key);
```

异步通知返回:

```php
#成功
return NotifyHandle::sendSuccessReturn();
#失败
return NotifyHandle::sendFailReturn("FAIL","ORDER_ERROR");
```

更多请参考 [微信支付分通用文档](https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/payscore/chapter1_1.shtml)。


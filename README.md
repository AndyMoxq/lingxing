# 领星OpenAPI PHP SDK

## 必须依赖

* ext-curl
* ext-json
* ext-openssl

## 安装

### 在项目中引入SDK
首先需要创建一个目录存放SDK的代码，如`ak_openapi`，然后将`src`里面的文件全部复制到这个新建的目录

### 开启自动加载
放好代码后，需要在项目根目录的`composer.json`文件中添加自动加载的命名空间，参照上方的目录命名，应该在`composer.json`中添加以下代码

```json
{
  //...
  "autoload": {
    "psr-4": {
      // ... 其他自动加载项
      "Ak\\OpenAPI\\": "ak_openapi"
    }
  }
  //...
}
```

添加好之后不要忘记执行一遍以下命令

```shell
composer dump-autoload
```

### 在项目中使用
完成上面两步之后，只需要参照`README`中的用法调用相关方法即可。

## API 总览

### 基本使用

```php
/**
 * host为OpenAPI域名，需要带上协议，如 https://openapi.lingxing.com
 * appId则为开发者的appId
 * appSecret为开发者的appSecret
 */
$client = new \Ak\OpenAPI\Services\OpenAPIRequestService('host', 'appId', 'appSecret');
/**
 * 发起请求前需要先生成AccessToken或手动设置AccessToken，否则会抛出 InvalidAccessTokenException
 * AccessToken有时效性，可以自行加入缓存，并判断是否已过期，方便续约或重新生成
 */
$accessTokenDto = $client->generateAccessToken();

/**
 * 获取AccessToken
 */
 $accessTokenDto->getAccessToken();
 
 /**
 * 获取RefreshToken（用于刷新AccessToken），请自行保存好
 */
 $accessTokenDto->getRefreshToken();

/**
 * 获取过期时间戳，请自行保存好，用于判断AccessToken是否已过期
 */
$accessTokenDto->getExpireAt();
 
 /**
  * 刷新AccessToken，AccessToken到期前需续约，这里请自行判断AccessToken的有效期
 */
 $client->refreshToken($accessTokenDto->getRefreshToken());

/**
 * 手动设置AccessToken
 */
 $accessToken = 'get_access_token_from_cache';
 $client->setAccessToken($accessToken);
 

/**
 * GET 请求示例
 * $res 会是一个数组，接口文档返回结果json_decode()后的数组结果
 */
$res = $client->makeRequest('/erp/sc/data/seller/lists', 'GET');

/**
 * POST 请求示例
 */
$params = ['start_date'=>'2023-07-18 00:00:00','end_date'=>'2023-08-18 23:59:59'];
$res = $client->makeRequest('/erp/sc/data/mws/orders', 'POST', $params);
```

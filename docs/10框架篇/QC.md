### <div id="接口请求频率限制"> 接口请求频率限制</div> 

为了防止恶意请求，所以在开发过程中通常会做接口请求频率限制：比如限制同一个用户(或IP)在一分钟内请求某个接口的次数。

laravel中使用redis首先要安装扩展:你需要通过 Composer 安装 predis/predis 扩展包：

```
composer require predis/predis
```

解决方案：当用户请求某个接口时，以用户身份和接口名称组合作为redis的一个key值。假如：用户名为zhangsan，接口名称为inter。在laravel中，代码如下：

```
Redis::setex('zhangsan_inter',60,1);
```

> 上面代码的含义为：key='zhangsan'，val=1，过期时间为一分钟

```
if(Redis::exists('zhangsan_inter')){
    Redis::incrby ('zhangsan_inter', 1);
}else{
    Redis::setex('zhangsan_inter',60,1);
}
$num = Redis::get('zhangsan_inter');
if($num>10){
    return '请求次数过于频繁';
}else{
    return 'success';
}
```

> 上面代码为一个demo，限制每个用户在一分钟内最多请求该接口10次。
>
> 在实际开发过程中，应该遵循面向对象的思想，将其封装成一个类，调用时，将key,过期时间，请求次数等作为参数传递。提高代码的复用性和可读性。简化开发流程。

###### 在laravel中调用redis方法与原生有所不同，具体的请参考：

[laravel中redis操作大全](https://blog.csdn.net/u010785091/article/details/80897517)

#### <div id="接口返回数据"> 接口请求频率限制</div> 

##### 一般来说接口返回格式分为数据部分和状态部分，如下：
```
public function outResource($data, $success = true, $error_code = "")
    {
        $status = $success ? "success" : "error";
        $result = [
            'status' => $status,
            'result' => $data
        ];
        if ($status == "error") {
            $result['result'] = [
                "error_msg"  => $data,
                'error_code' => $error_code
            ];
        }
        return $result;
    }
```
返回数据时调用这个方法即可。

#### <div id="curl"> CURL请求接口</div> 

```php
/**
 * HTTP POST 请求
 * @param string $url
 * @param array $data
 *
 * @return mixed
 */
function httpPostRequest($url, $data = [], $header = [])
  if (is_array($data)) {
        $data = http_build_query($data);
    }
    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 30);
    curl_setopt($ch, CURLOPT_TIMEOUT, 50);
    if (!empty($header)) {
        curl_setopt($ch, CURLOPT_HTTPHEADER, $header);
    }
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

    //https请求
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, FALSE);
    $result = curl_exec($ch);
    curl_close($ch);
    return $result;
  
  }

/**
 * HTTP GET 请求
 * @param string $url
 * @return mixed
 */
function httpGetRequest($url)
{
    $curl = curl_init();
    curl_setopt($curl, CURLOPT_URL, $url);
    curl_setopt($curl, CURLOPT_HEADER, 0);
    curl_setopt($curl, CURLOPT_CONNECTTIMEOUT, 30);
    curl_setopt($curl, CURLOPT_TIMEOUT, 50);
    curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
    $data = curl_exec($curl);
    curl_close($curl);
    return $data;
}
  
```


    //https请求
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, FALSE);
    $result = curl_exec($ch);
    curl_close($ch);
    return $result;
}
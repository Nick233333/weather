<h1 align="center"> weather </h1>

<p align="center">:rainbow: 基于高德开放平台的 PHP 天气信息组件。</p>

![Build Status](https://travis-ci.org/Nick233333/weather.svg?branch=master)
![StyleCI build status](https://github.styleci.io/repos/181275329/shield) 

## 安装

```shell
$ composer require nick233333/weather -vvv
```

## 配置

在使用本扩展之前，你需要去 [高德开放平台](https://lbs.amap.com/dev/id/newuser) 注册账号，然后创建应用，获取应用的 API Key。

## 使用

```php
use Nick233333\Weather\Weather;

$key = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx';

$weather = new Weather($key);
```

###  获取实时天气

```php
$response = $weather->getLiveWeather('深圳');
```
示例：

```json
{
    "status": "1",
    "count": "1",
    "info": "OK",
    "infocode": "10000",
    "lives": [
        {
            "province": "广东",
            "city": "深圳市",
            "adcode": "440300",
            "weather": "阴",
            "temperature": "23",
            "winddirection": "西北",
            "windpower": "≤3",
            "humidity": "85",
            "reporttime": "2019-05-01 09:44:31"
        }
    ]
}
```

### 获取近期天气预报

```
$response = $weather->getForecastsWeather('深圳');
```
示例：

```json

{
    "status": "1",
    "count": "1",
    "info": "OK",
    "infocode": "10000",
    "forecasts": [
        {
            "city": "深圳市",
            "adcode": "440300",
            "province": "广东",
            "reporttime": "2019-05-01 09:44:31",
            "casts": [
                {
                    "date": "2019-05-01",
                    "week": "3",
                    "dayweather": "阴",
                    "nightweather": "阴",
                    "daytemp": "28",
                    "nighttemp": "22",
                    "daywind": "无风向",
                    "nightwind": "无风向",
                    "daypower": "≤3",
                    "nightpower": "≤3"
                },
                {
                    "date": "2019-05-02",
                    "week": "4",
                    "dayweather": "阴",
                    "nightweather": "多云",
                    "daytemp": "27",
                    "nighttemp": "22",
                    "daywind": "无风向",
                    "nightwind": "无风向",
                    "daypower": "≤3",
                    "nightpower": "≤3"
                },
                {
                    "date": "2019-05-03",
                    "week": "5",
                    "dayweather": "多云",
                    "nightweather": "多云",
                    "daytemp": "28",
                    "nighttemp": "21",
                    "daywind": "无风向",
                    "nightwind": "无风向",
                    "daypower": "≤3",
                    "nightpower": "≤3"
                },
                {
                    "date": "2019-05-04",
                    "week": "6",
                    "dayweather": "多云",
                    "nightweather": "阵雨",
                    "daytemp": "28",
                    "nighttemp": "24",
                    "daywind": "无风向",
                    "nightwind": "无风向",
                    "daypower": "≤3",
                    "nightpower": "≤3"
                }
            ]
        }
    ]
}
```

### 获取 XML 格式返回值

以上两个方法第二个参数为返回值类型，可选 `json` 与 `xml`，默认 `json`：

```php
$response = $weather->getLiveWeather('深圳', 'xml');
```

示例：

```xml
<?xml version="1.0" encoding="utf-8"?>

<response>
  <status>1</status>
  <count>1</count>
  <info>OK</info>
  <infocode>10000</infocode>
  <lives type="list">
    <live>
      <province>广东</province>
      <city>深圳市</city>
      <adcode>440300</adcode>
      <weather>阴</weather>
      <temperature>23</temperature>
      <winddirection>西北</winddirection>
      <windpower>≤3</windpower>
      <humidity>85</humidity>
      <reporttime>2019-05-01 09:44:31</reporttime>
    </live>
  </lives>
</response>
```

### 参数说明

```
array | string   getLiveWeather(string $city, string $format = 'json')
array | string   getForecastsWeather(string $city, string $format = 'json')
```

> - `$city` - 城市名/[高德地址位置 adcode](https://lbs.amap.com/api/webservice/guide/api/district)，比如：“深圳” 或者（adcode：440300）；
> - `$format`  - 输出的数据格式，默认为 json 格式，当 output 设置为 “`xml`” 时，输出的为 XML 格式的数据。


### 在 Laravel 中使用

在 Laravel 中使用也是同样的安装方式，配置写在 `config/services.php` 中：

```php
    .
    .
    .
     'weather' => [
        'key' => env('WEATHER_API_KEY'),
    ],
```

然后在 `.env` 中配置 `WEATHER_API_KEY` ：

```env
WEATHER_API_KEY=xxxxxxxxxxxxxxxxxxxxx
```

可以用两种方式来获取 `Overtrue\Weather\Weather` 实例：

#### 方法参数注入

```php
    .
    .
    .
    public function edit(Weather $weather) 
    {
        $response = $weather->getLiveWeather('深圳');
    }
    .
    .
    .
```

#### 服务名访问

```php
    .
    .
    .
    public function edit() 
    {
        $response = app('weather')->getLiveWeather('深圳');
    }
    .
    .
    .

```

## 参考

- [高德开放平台天气接口](https://lbs.amap.com/api/webservice/guide/api/weatherinfo/)

## License

MIT
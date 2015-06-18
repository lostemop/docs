title: 数据统计与分析
---

通过数据接口，开发者可以获取与公众平台官网统计模块类似但更灵活的数据，还可根据需要进行高级处理。

本 SDK 由 `Overtrue\Wechat\Stats` 提供微信数据统计查询服务。

> 1. 接口侧的公众号数据的数据库中仅存储了 **2014年12月1日之后**的数据，将查询不到在此之前的日期，即使有查到，也是不可信的脏数据；
> 2. 请开发者在调用接口获取数据后，将数据保存在自身数据库中，即加快下次用户的访问速度，也降低了微信侧接口调用的不必要损耗。
> 3. 额外注意，获取图文群发每日数据接口的结果中，只有**中间页阅读人数+原文页阅读人数+分享转发人数+分享转发次数+收藏次数 >=3** 的结果才会得到统计，过小的阅读量的图文消息无法统计。

### 获取实例

```php
<?php

use Overtrue\Wechat\Stats;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$stats = new Stats($appId, $secret);
```

## API

    $from   example: `2014-02-13` 获取数据的起始日期
    $to     example: `2014-02-18` 获取数据的结束日期，`$to`允许设置的最大值为昨日

    `$from` 和 `$to` 的差值需小于 “最大时间跨度”（比如最大时间跨度为 1 时，`$from` 和 `$to` 的差值只能为 0，才能小于 1 ），否则会报错

+ `array userSummary($from, $to)` 获取用户增减数据, 最大时间跨度：**7**;
+ `array userCumulate($from, $to)` 获取累计用户数据, 最大时间跨度：**7**;
+ `array articleSummary($from, $to)` 获取图文群发每日数据, 最大时间跨度：**1**;
+ `array articleTotal($from, $to)` 获取图文群发总数据, 最大时间跨度：**1**;
+ `array userReadSummary($from, $to)` 获取图文统计数据, 最大时间跨度：**3**;
+ `array userReadHourly($from, $to)` 获取图文统计分时数据, 最大时间跨度：**1**;
+ `array userShareSummary($from, $to)` 获取图文分享转发数据, 最大时间跨度：**7**;
+ `array userShareHourly($from, $to)` 获取图文分享转发分时数据, 最大时间跨度：**1**;
+ `array upstreamMesssageSummary($from, $to)` 获取消息发送概况数据, 最大时间跨度：**7**;
+ `array upstreamMesssageHourly($from, $to)` 获取消息分送分时数据, 最大时间跨度：**1**;
+ `array upstreamMesssageWeekly($from, $to)` 获取消息发送周数据, 最大时间跨度：**30**;
+ `array upstreamMesssageMonthly($from, $to)` 获取消息发送月数据, 最大时间跨度：**30**;
+ `array upstreamMesssageDistSummary($from, $to)` 获取消息发送分布数据, 最大时间跨度：**15**;
+ `array upstreamMesssageDistWeekly($from, $to)` 获取消息发送分布周数据, 最大时间跨度：**30**;
+ `array upstreamMesssageDistMonthly($from, $to)` 获取消息发送分布月数据, 最大时间跨度：**30**;
+ `array interfaceSummary($from, $to)` 获取接口分析数据, 最大时间跨度：**30**;
+ `array interfaceSummaryHourly($from, $to)` 获取接口分析分时数据, 最大时间跨度：**1**;

example:

```php
$userSummary = $stats->userSummary('2014-12-07', '2014-12-08');

var_dump($userSummary);
//
//[
//    {
//        "ref_date": "2014-12-07",
//        "user_source": 0,
//        "new_user": 0,
//        "cancel_user": 0
//    }
//    //后续还有ref_date在begin_date和end_date之间的数据
// ]

```

更多详细内容与协议说明，请查看微信官方文档：http://mp.weixin.qq.com/wiki/ **数据统计** 章节
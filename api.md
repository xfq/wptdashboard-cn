# wpt.fyi API

English version: https://github.com/web-platform-tests/wpt.fyi/blob/master/api/README.md

本文档介绍 wpt.fyi 的 HTTP 端点的用法和参数。

## /api/results/upload

该端点能够上传 wptreport 的结果。

此端点仅接受 POST 请求，并需要通过 HTTP 基本身份验证对请求进行身份验证。如果您想注册为“测试运行者”，请联系 [Ecosystem Infra](mailto:ecosystem-infra@chromium.org) 上传结果。

### File payload

__Content type__: `multipart/form-data`

__参数__

__`result_file`__: `wpt run --log-wptreport` 生成的 gzipped JSON 文件。

__`labels`__: （可选）一个字符串，内容以逗号分隔，包含此测试运行的相关标签。目前支持的标签有“experimental”和“stable”（被测试的浏览器的版本）。

JSON 文件大致如下所示：

```json
{
  "results": [...],
  "time_start": Unix时间（微秒）,
  "time_end": Unix时间（微秒）,
  "run_info": {
    "revision": "测试运行的 WPT 版本",
    "product": "你的浏览器",
    "browser_version": "浏览器版本",
    "os": "你的操作系统",
    "os_version": "（可选）操作系统版本",
    ...
  }
}
```

__Notes__

`time_start` 和 `time_end` 字段是整个测试运行开始和结束时的数字时间戳（自UNIX纪元以来的微秒）。这两个参数是可选的（但是推荐包含），`wpt run` 也会在在报告中默认生成这两个参数。

JSON 文件必须包含 `run_info.{revision,product,browser_version,os}` 字段，这些字段也会由 `wpt run` 自动生成。

### URL payload

__Content type__: `application/x-www-form-urlencoded`

__参数__

TODO

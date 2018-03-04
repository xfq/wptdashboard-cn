# web-platform-tests dashboard for Chinese browsers

运行 [web-platform-tests](https://github.com/w3c/web-platform-tests) 的测试结果。

## 包含的数据

一共有两种类型的 gzipped JSON 数据文件：测试运行摘要文件和单个测试结果文件。

### 测试运行摘要文件

格式：`{sha[0:10]}/{platform_id}-summary.json.gz`

- `sha[0:10]`: WPT commit hash的前10个字符
- `platform_id`: `webapp/browsers.json` 中的键，包含浏览器和运行平台

样例：[791e95323d/firefox-56.0-linux-summary.json.gz](https://storage.googleapis.com/wptd/791e95323d/firefox-56.0-linux-summary.json.gz)

结构：
一个对象，其中的键是测试文件名，值是 `[number passing subtests, total number subtests]` 类型的列表。

```json
{
    "/test/file/name1.html": [0, 1],
    "/test/file/name2.html": [5, 10]
}
```

### 单个测试结果文件

格式：`{sha[0:10]}/{platform_id}/{test_file_path}`

- `sha[0:10]`: WPT commit hash的前10个字符
- `platform_id`: `webapp/browsers.json` 中的键，包含浏览器和运行平台
- `test_file_path`: 测试文件在 web-platform-tests 目录里的路径

样例：[b12daf6ead/safari-10-macos-10.12-sauce/IndexedDB/abort-in-initial-upgradeneeded.html](https://storage.googleapis.com/wptd/b12daf6ead/safari-10-macos-10.12-sauce/IndexedDB/abort-in-initial-upgradeneeded.html)

结构：
```json
{
    "test": "/test/file/name.html",
    "status": "OK",
    "message": "失败消息（如果存在）",
    "subtests": [
        {
            "status": "FAIL",
            "name": "子测试名称",
            "message": "失败消息（如果存在）"
        }
    ]
}
```

## 感谢

* [wptdashboard](https://github.com/w3c/wptdashboard/)

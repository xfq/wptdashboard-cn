Web 平台测试项目 [![IRC chat](https://goo.gl/6nCIks)](http://irc.w3.org/?channels=testing)
==============================

最新版请参看原文: https://github.com/web-platform-tests/wpt#the-web-platform-tests-project

Web 平台测试项目是各大浏览器厂商在 W3C 整合的为 Web 平台搭建一套跨浏览器的测试库的一个尝试。如果开发阶段所写的测试能在不同浏览器等效运行，浏览器项目则有信心其所发布的软件与其它浏览器实现是一致的，而且以后的实现者也能和其当前实现保持一致。同时这也给予 Web 开发者信心 Web 平台所发布的接口是可依赖的、严格遵循标准实现的、可跨浏览器和设备运行的，不需要前缀或额外的非规范代码。

项目初始化
===================

最新版请参看原文：https://github.com/web-platform-tests/wpt#setting-up-the-repo

请通过 https://github.com/web-platform-tests/wpt 克隆或获取项目。

注：因本 repo 创建删除 branch 均非常频繁，推荐在更新时 "prune” 过期的 branch，即使用 `git pull --prune` (或 `git fetch -p && git merge`).

如何运行测试
=================

最新版请参看原文：https://github.com/web-platform-tests/wpt#running-the-tests

这些测试用例的设计初衷都是运行在本地环境。测试环境需要  [Python 2.7+](http://www.python.org/downloads) (注意非 Python 3.x).

在 Windows, 请记得添加 Python 文件夹 (`c:\python2x`, 默认的) 到你的 `%Path%` [环境变量](http://www.computerhope.com/issues/ch000549.htm),
并请阅读 WPT 的 [Windows Notes](https://github.com/web-platform-tests/wpt#windows-notes) 章节.

运行测试前, 请配置测试域名
[`hosts` 文件](http://en.wikipedia.org/wiki/Hosts_%28file%29%23Location_in_the_file_system).

相关内容可以通过 `./wpt make-hosts-file` 指令生成; 在
Windows, 你需要执行预指令 `python` 或指定 python 安装版本的指令 (`python wpt make-hosts-file`).

例如, 在大多数 UNIX-like 系统, 你可以通过以下指令配置 hosts file:

```bash
./wpt make-hosts-file | sudo tee -a /etc/hosts
```

而在 Windows (必须运行在 Administrator 权限的 PowerShell 会话):

```bash
python wpt make-hosts-file | Out-File %SystemRoot%\System32\drivers\etc\hosts -Encoding ascii -Append
```

如果你使用了 proxy, 你必须保证上述域名不在 proxy lookup 的范围内.


手动运行测试
======================

最新版请参看原文：https://github.com/web-platform-tests/wpt#running-the-tests

通过以下指令启动 test server
```
./wpt serve
```

**在 Windows**: 你需要运行预指令 `python` 或指定 python 安装版本的指令
```bash
python wpt serve
```

这会在两个端口启动 HTTP 服务器而且在一个端口启动 websockets 服务器。默认状态下 Web server 开启于 8000 和 8443 端口，其它端口随机在闲置端口选择。测试会通过 *第一* 个输出的 HTTP server 下载。 若要改变端口,
请在 wpt 根目录创建一个 `config.json` 文件, 并添加你自定义的端口，例如:

```
{
  "ports": {
    "http": [1234, "auto"],
    "https":[5678]
  }
}
```

`hosts` 文件配置完毕, server 即可通过以下地址访问:

http://web-platform.test:8000/<br>
https://web-platform.test:8443/ *

\**请参看 [Trusting Root CA](https://github.com/web-platform-tests/wpt#trusting-root-ca)*

自动运行测试
---------------------------

最新版请参看原文：https://github.com/web-platform-tests/wpt#running-tests-automatically

测试可以通过 `wpt` 脚本的 `run` 直接在浏览器运行。 运行这个指令需要配置好上述的 hosts
文件, 请 *不* 要在运行 `wpt run` 前单独启动 test server. 运行测试的基本指令语法是:

```bash
./wpt run product [tests]
```

**在 Windows**: 你需要运行预指令 `python` 或指定 python 安装版本的指令。
```bash
python wpt run product [tests]
```

这里的 `product` 现在可以指定为 `firefox` 或 `chrome` ，而 `[tests]` 可以列出相关测试用例的路径. 这个指令会尝试定位一个浏览器实例并且启动所需的 dependencies. 这个指令的配置灵活度很高： 例如可以指定一个 binary，通过
`wpt run --binary=path product`. 想查看所有的配置项，请尝试 `wpt run --help` 和 `wpt run --wptrunner-help`.

不是所有的 dependencies 都可以自动启动; 特别是在 Firefox 下启动 https 所必需的
`certutil` 工具，必须通过对应的 system package manager 或类似工具启动。

在 Debian/Ubuntu certutil 的启动必须使用:

```
sudo apt install libnss3-tools
```

而在 macOS 在配置好 homebrew 的情况下请用:

```
brew install nss
```

在其它平台, 你有可能需要通过 [Mozilla CI](https://archive.mozilla.org/pub/firefox/nightly/latest-mozilla-central/) 下载 firefox archive 和 common.tests.zip.

然后解压 tests.zip package 和 `libnss3[.so|.dll|.dynlib]` 得到 `certutil[.exe]` 并把前者放在你的 path，后者放在 library path.

在移动浏览器运行测试
---------------------------

请参考：https://chromium.googlesource.com/chromium/src/+/master/docs/testing/web_platform_tests.md#Running-tests
和 
https://github.com/web-platform-tests/wpt.fyi
以及
https://web-platform-tests.org/running-tests/chrome_android.html

如何生成测试结果
===================

每个浏览器平台的配置细节可能不同，请参考 wpt.fyi 项目说明： https://github.com/web-platform-tests/wpt.fyi#setting-up-your-environment

环境配置
---------------------------

你可能需要 [Docker](https://www.docker.com/). Docker 安装后, 创建 base image 和 development image, 并且启动一个 development server 实例:

```sh
docker build -t wptd-dev .
./util/docker-dev/run.sh
```

此指令会启动一个 Docker 示例，名为 `wptd-dev-instance`.

本地运行
---------------------------

一旦实例开始运行，你可以在另一个 terminal 启动 web server:

```sh
./util/docker-dev/web_server.sh
```

此指令将 build dependencies 并在 `wptd-dev-instance` 里启动 the Google App Engine development server.

webserver 运行时, 你需要往 app datastore 添加一些初始数据. 请在另一个 terminal 里,
运行以下指令，执行脚本配置 `util/populate_dev_data.go`:

```sh
./util/docker-dev/dev_data.sh
```

文件系统和网络输出 (in Chrome Android)
---------------------------

- 这个脚本会在 `config['build_path']` 写入文件.
- 一次运行会写入约 111MB 到文件系统.
- 如果指定了 --upload, 会上传 111MB 结果到 GCS.
- 要上传结果，你可能要用 `gcloud` 在 `wpt.fyi` 登陆.

使用数据
---------------------------

所有的测试结果都是公开的。一些是我们保存的两种形式的 gzipped JSON 数据: test run 总结数据, 和单独的 test result 文件.

test run 总结数据
---------------------------

pattern: `{sha[0:10]}/{platform_id}-summary.json.gz`

- `sha[0:10]`: 所运行的 WPT commit hash 的头 10 个字符
- `platform_id`: `webapp/browsers.json`的 platform configuration key

例子: https://storage.googleapis.com/wptd/791e95323d/firefox-56.0-linux-summary.json.gz

(注 `wptd` 是 bucket 的名字)

结构:
一个 object，其 key 是 test 的文件名，value 是一个列表，元素为
`[number passing subtests, total number subtests]`.

```json
{
    "/test/file/name1.html": [0, 1],
    "/test/file/name2.html": [5, 10]
}
```

单独的 test result 文件
---------------------------

pattern: `{sha[0:10]}/{platform_id}/{test_file_path}`

- `sha[0:10]`: 所运行的 WPT commit hash 的头 10 个字符
- `platform_id`: `webapp/browsers.json`的 platform configuration key
- `test_file_path`: test file 的全路径

例子: https://storage.googleapis.com/wptd/b12daf6ead/safari-10-macos-10.12-sauce/IndexedDB/abort-in-initial-upgradeneeded.html

结构:
```json
{
    "test": "/test/file/name.html",
    "status": "OK",
    "message": "The failure message, if exists",
    "subtests": [
        {
            "status": "FAIL",
            "name": "The subtest name",
            "message": "The failure message, if exists"
        }
    ]
}
```

如何在公布板注册
===================

/api/results/upload

需要上传一份 wptreport 到 wpt dashboard.

终端只能接受 POST 请求. 请求需要通过一个 HTTP basic auth 授权.
请联系 [Ecosystem Infra](mailto:ecosystem-infra@chromium.org) 如果您希望注册为
"test runner" 的一员, 上传测试结果.

如何提交测试结果
===================

请参考对应的 API https://github.com/web-platform-tests/wpt.fyi/blob/master/api/README.md

项目 license
===================

WPT项目遵循  [W3C Test Suite License 和 W3C 3-clause BSD License](https://github.com/web-platform-tests/wpt/blob/master/LICENSE.md)

wpt.fyi项目 遵循 [W3C 3-clause BSD License](https://github.com/web-platform-tests/wpt.fyi/blob/master/LICENSE)

# Results dashboard for web-platform-tests

## URL

* https://wpt.fyi/
* https://github.com/w3c/wptdashboard

## 使用的技术

* [Google App Engine](https://cloud.google.com/appengine/)
* [Docker](https://www.docker.com/)
* [Protocol Buffers](https://github.com/google/protobuf)
* [Polymer](https://www.polymer-project.org/)

## 代码结构

### `run/`

运行测试、生成结果和将上传结果的相关代码。

### `util/`

在 Docker 环境里启动 Web 服务器、填充测试结果数据等工具代码。

### `webapp/`

处理、加载和展示测试结果数据的代码。

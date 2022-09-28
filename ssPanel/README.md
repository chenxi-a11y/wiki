# 概述

**官方教程地址（没有宝塔教程）**[https://wiki.sspanel.org/](https://wiki.sspanel.org/#/)

SSPanel-UIM 是一个典型的 PHP 网页应用程序，它需要以下服务器程序才能正常工作

- HTTP 服务器。通常是 Apache 或者是 Nginx，出于性能考虑我们推荐用户使用 Nginx 部署。
- PHP 脚本运行程序。目前版本的 SSPanel-UIM 是基于 PHP 8.0 版本进行开发和测试的。

- 类 MySQL 数据库。这里包括 Oracle MySQL 和 MariaDB，我们推荐使用最新版本的 MariaDB。

在代码层面，SSPanel-UIM 使用 Slim Framework 3 作为后端的基础框架，使用 Smarty 模板引擎提供前端渲染，Composer 管理第三方组件。 


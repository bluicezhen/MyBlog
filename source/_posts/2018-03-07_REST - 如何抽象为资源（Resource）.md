---
title: 如何抽象为资源（Resource）
date: 2018-03-07 21:39:07
tags: 
- REST
- HTTP
- 设计风格
summary: HTTP 协议最初的目的是为了提供一种发布和接收 HTML 页面的方法。HTTP 1.0 之前的版本，支持的 Method 只有GET，用于获取用 URI 定义的 HTML 文件。
---

HTTP 协议最初的目的是为了提供一种发布和接收 HTML 页面的方法。HTTP 1.0 之前的版本，支持的 Method 只有 `GET`，用于获取用 URI 定义的 HTML 文件。

**URI（Uniform Resource Identifiers）**的含义是**“统一资源定位符”**，即从 HTTP 诞生之初，就是围绕着**资源（Resource）**进行的。

2000年 Roy Thomas Fielding 发表著名论文[《架构风格与基于网络的软件设计》](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)，被认为是首次提出了 **REST（Representational State Transfer）** 的概念。Fielding 本人也参与了 HTTP 的设计工作，说 HTTP 1.0/1.1 协议是 REST 的实践者一点都不过分。

在 RESTful API 设计中，重点在于如何将 WEB 应用中的万事万物都抽象为**资源**，这个**面向对象编程**是有异曲同工之妙的。

就我个人而言，**第一阶段**作为一个 PHP 初学者，所有操作是基于页面跳转/提交的。例如登录即先访问 `/login.php`，在这个页面填写一个表单，使用 `POST` 方法提交到 `/login_post.php`，在 `login_post.php` 有判断，成功则跳转至用户首页 `/my_homepage.php`，此时，在 `my_homepage.php` 已可以根据 PHP 内置变量 `$_SESSION` 查询数据库获取当前用户信息，展示丰富的页面。

这种方法很明显是非常容易把展示性页面可功能型页面混淆，而且命名及其困难。

到了**第二阶段**，开始使用 `Ajax` 传递数据（第二阶段对 SPA 和 SSR 是一样的）, 依然是访问 `/login.php` 填写提交表单，但不再是一个页面，而是一个 API。我会写类似的 Jeuery 代码：

```javascript
$.post (
    "/api/login.php",
    {
        username: "user1",
        password: "123456"
    },
    function (data, status) {
        if (200 <= status < 300 && data.success) {
            alert("Login Success");
            location.href = "/my_homepage.php";
        } else {
            alert("Login Field");
        }
    }
);
```

实际上，在这个阶段我们实现了一种非常非常蹩脚的 JSON-RPC 协议。RPC 的主要思想是把函数映射到 API ，对应 `login.php` 就应该是一个抽象函数的实例，退出登录也是一个函数 `logout.php`。即：

```php
function login ($username, $password) -> Json;
function logout () -> Json;
```

我一开始觉得登录、注销等操作抽象为资源很困难，主要原因是 PHP 、 Python Flask 这些工具已经屏蔽了 Session 实现的细节，我并不了解登录、注销等操作的本质。

对于 HTTP 协议来说，其本身是无状态的，即使 HTTP/1.1 加入了 `Connection: keep-alive`，但不能做到真正的长连接。于是大家想出了很多办法来保存用户的登录状态。从最早的不靠谱的 在 URL 参数里写入 Token 实现到 Cookie 到 Storage 甚至 IndexDB。

PHP 默认使用 Cookie 实现 Session，很多 PHP 网站返回的 HTTP 报文里都会带有一个 `Cookie:PHPSESSID=xxx` 的 HTTP Hearder。 PHP 就是靠这个 PHPSESSID 来判断用户身份的。同时 PHP 会在服务器磁盘上留下相关文件，来持久化存储。

划重点：**用户登录 = 创建标识符**、**用户注销 = 删除标识符**

这里资源即**用户标识符**，支持创建、删除操作

创建（登录）：

```text
POST /api/token.php HTTP/1.1
Host: exapmle.com
Content-Type: application/json

{
    "username": "user1",
    "password": "123456"
}
```

```text
POST /user_access_token HTTP/1.1
Host: exapmle.com
Content-Type: application/json
Cookie:PHPSESSID=123456789

{
    "msg": "登录成功",
    "success": true
}
```

删除（注销）：

```text
DELETE /api/token.php HTTP/1.1
Host: exapmle.com
Content-Type: application/json
Cookie:PHPSESSID=123456789

{
    "msg": "注销成功"，
    "success": true
}
```

我们也可以不使用 PHP 自带的 Seesion 功能，也可以自己实现，本文不在不说。


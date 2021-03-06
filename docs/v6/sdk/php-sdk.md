---
layout: docs
title: PHP SDK 使用指南
---
<a id="overview"></a>
# Qiniu Storage SDK for PHP

此 SDK ( version: 7.x.x ) 适用于 PHP 5.3 及其以上版本。基于 [七牛云存储官方API](../index.html) 构建。使用此 SDK 构建您的网络应用程序，能让您以非常便捷地方式将数据安全地存储到七牛云存储上。无论您的网络应用是一个网站程序，还是包括从云端（服务端程序）到终端（手持设备应用）的架构的服务或应用，通过七牛云存储及其 SDK，都能让您应用程序的终端用户高速上传和下载，同时也让您的服务端更加轻盈。

此 SDK 包含了如下几个功能: 

    数据的上传和下载。 

    数据的管理: 复制，移动，删除，获取元信息，列取文件。  

    数据的处理: 图片的处理，音视频的处理，文档的处理。

本SDK所提供的所有API的详细使用说明: [API参考手册](http://developer.qiniu.com/php_docs/index.html)

SDK(version: 6.x.x 适用于PHP >= 5.1.0): [相关文档](http://developer.qiniu.com/docs/v6/sdk/legacy-php-sdk.html)

- [概述](#overview)
- [快速入门](#quick-start)
  - [安装](#install)
  - [SDK 使用](#usage)
- [场景化示例](#demos)
  - [Hello World](#hello-world)
  - [移动应用](#android-demo)
  - [图片处理](#image-proc)
  - [音视频处理](#av-proc)
- [FAQ](#faq)
- [相关资源](#related-resources)

<a id="quick-start"></a>
# 快速入门
- - -

<a id="install"></a>
## 安装

支持以下几种安装方式。推荐使用 **Composer**安装方式 。

<a id="install-composer"></a>
### 使用 Composer 安装

**[Composer](http://getcomposer.org/)** 是一个简单易用的 PHP 依赖管理工具。你可以在自己的项目中声明所依赖的外部工具库，Composer 会自动帮你安装这些依赖的库文件。

使用 Composer 安装 Qiniu SDK 的步骤非常简单：

1. 安装 Composer :

    ```
    curl -sS https://getcomposer.org/installer | php
    ```

2. 使用 Composer 获取最新版本的 Qiniu SDK :

    ```
    php composer.phar require qiniu/php-sdk
    ```

3. 你代码中 `require`  Composer生成的 autoloader：

    ```
    <?php
    require 'vendor/autoload.php';
    ```

如果需要更详细的关于 Composer 的使用说明，你可以访问 Composer 的官方网站[http://getcomposer.org/](http://getcomposer.org/)，或对应的中文网站 [http://www.phpcomposer.com/](http://www.phpcomposer.com/)。

<a id="install-composer-pkg"></a>
### 直接下载 Composer 安装包

鉴于某些原因, 国内的用户使用 Composer 下载依赖库比较慢，开发者也可以直接下载我们准备好的符合 Composer 规范的 vendor 压缩包并在本地解压。解压后的内容和使用 Composer 方式获取到的内容完全一致，之后的使用方式也一致。

压缩包下载地址：[http://devtools.qiniu.io/vendor.tar.gz](http://devtools.qiniu.io/vendor.tar.gz)


<a id="src-pkg"></a>
### 下载源代码包

直接下载源代码压缩包并解压是另一种 SDK 的安装方法。不过因为有版本更新的维护问题，这种安装方法并不推荐，仅作为万一 Composer 安装有问题的情况下作为一种选择。
并且，源码中也包含 Qiniu SDK 的 [autoloader](https://github.com/qiniu/php-sdk/blob/master/autoload.php)。

下载[源代码包](https://github.com/qiniu/php-sdk/releases)后，解压到您的项目中。 然后在您的项目中引入 autoloader:
 
```
  <?php
  require 'path_to_sdk/autoload.php';
```
即可以使用七牛相关功能。

<a id="usage"></a>
## SDK 使用

安装好SDK后接下来我们介绍下如何使用该SDK。 在使用SDK之前，你必须先[注册一个七牛帐号](https://portal.qiniu.com/signup)，并登录控制台获取一对有效的[AccessKey和SecretKey](https://portal.qiniu.com/setting/key), 并且详细阅读[安全建议](http://developer.qiniu.com/docs/v6/api/reference/security/)以进一步了解如何正确使用和管理密钥。

<a id="usage-require"></a>
### 引入 SDK
无论你是通过哪种方式安装SDK， 安装之后必须将SDK包含到你的项目中才可以调用SDK中的方法。你可以参考以下的表格，按照你的安装方式将SDK引入到你的项目中。只需要一句简单的 `require` 将autoloader引入即可使用，注意要将 `path_to_sdk` 替换成你项目中的实际路径。

安装方式     |  引入SDK
:---------- | :------------
Composer 安装 | `require 'path_to_sdk/vendor/autoload.php';`
下载Composer安装包 | `require 'path_to_sdk/vendor/autoload.php';`
下载源码安装包 | `require 'path_to_sdk/autoload.php';`

<a id="usage-examples"></a>
### 结构&示例
可以直接看我们[SDK的目录](https://github.com/qiniu/php-sdk/tree/master/src/Qiniu), 即可大致了解我们SDK的结构。

* 最外层包含：配置文件， 鉴权，一些公共的函数。 
* `HTTP`目录主要包含了一些对http进行封装类， 这块你可以不用关心。
* `Storage`目录主要包含两大块：`Bucket中文件的管理` 和 `文件的上传`。  
* `Processing`目录主要包含文件的处理，文件处理又包含两个方面：同步处理和异步处理。

为了更好的了解和使用我们的sdk， 我们提供了各种[使用示例](https://github.com/qiniu/php-sdk/tree/master/examples)。 如果使用遇到问题，可以在[Github](https://github.com/qiniu/php-sdk/issues/new) 或者我们的[工单](http://support.qiniu.com/hc/) 进行反馈。下面我们对一些关键点进行简单的介绍， 以便可以快速入门。

<a id="usage-auth"></a>
### 鉴权
在使用七牛的SDK的过程中签权是很重要的一块， 不管是上传鉴权，下载签权， 还是回调的签权。 PHP SDK 中的[Auth](http://developer.qiniu.com/php_docs/class-Qiniu.Auth.html)类封装了所有的签权方式。
所以在使用 PHP SDK 时基本都会先对授权类进行初始化：

```php
<?php
  require 'path_to_sdk/vendor/autoload.php';
  use Qiniu\Auth;

  $accessKey = 'Access_Key';
  $secretKey = 'Secret_Key';

  $auth = new Auth($accessKey, $secretKey);
```

初始化好 `Auth` 类之后， 就可以调用相应的方法了， 比如生成私有下载链接方法 `privateDownloadUrl`, 获取上传Token方法 `uploadToken`, 验证回调方法`verifyCallback` 等。

<a id="usage-upload"></a>
### 上传
七牛的上传分为两种上传方式：`直接上传` 和 `分片上传` 。 当然在PHP SDK中你不用关心这个细节， 只需要调用 [UploadManager](https://github.com/qiniu/php-sdk/blob/master/src/Qiniu/Storage/UploadManager.php)中的上传方法即可。
UploadManager 中的上传方法会根据上传文件的大小来选择不同的上传方式， 小于4MB的进行直传， 大于4MB分片上传。以下一个简单的示例使你更清晰地了解七牛的上传：

```php
 <?php
    require_once 'path_to_sdk/vendor/autoload.php';

    // 引入鉴权类
    use Qiniu\Auth;

    // 引入上传类
    use Qiniu\Storage\UploadManager;

    // 需要填写你的 Access Key 和 Secret Key
    $accessKey = 'Access_Key';
    $secretKey = 'Secret_Key';
    
    // 构建鉴权对象
    $auth = new Auth($accessKey, $secretKey);
    
    // 要上传的空间
    $bucket = 'Bucket_Name';

    // 生成上传 Token
    $token = $auth->uploadToken($bucket);
    
    // 要上传文件的本地路径
    $filePath = './php-logo.png';

    // 上传到七牛后保存的文件名
    $key = 'my-php-logo.png';

    // 初始化 UploadManager 对象并进行文件的上传。
    $uploadMgr = new UploadManager();
    list($ret, $err) = $uploadMgr->putFile($token, $key, $filePath);
    echo "\n====> putFile result: \n";
    if ($err !== null) {
        var_dump($err);
    } else {
        var_dump($ret);
    }
```
<a id="demos"></a>
# 场景化示例
- - -

接下来我们，对一些常见的使用场景，提供一些简单的Demo. 使您更快速的接入七牛。

<a id="hello-world"></a>
## Hello, World!
在了解了基本概念和编程模型后，我们现在以最短的篇幅将第一个云存储服务的示例程序跑起来。该示例只有一个最简单的目标：将一个本地小文件上传到指定的存储空间中。

<a id="hw-submit-page"></a>
### 提交页面

我们先写一个简单的 HTML 表单来供用户上传到七牛存储空间信息并选择一个本地文件。在点击提交按钮后，浏览器会发送一个 HTTP 请求到七牛上传域名执行文件上传动作。


```html
<form method="post" action="http://up.qiniu.com" enctype="multipart/form-data">
  <input name="token" type="hidden" value="<upload_token>">
  <input name="file" type="file" />
  <input type="submit" value="上传"/>
</form>  
```
<a id="hw-upload"></a>
### 上传动作

也许你注意到了这个上传表单有一个`token`字段， 所以想要使用form直传七牛一个文件，必须在页面加载的时候，向服务端请求`token`并设置到这个表单项中。服务端生成这个`token`涉及到我们 SDK 中提供的方法：[`Qiniu\Auth`](http://developer.qiniu.com/php_docs/class-Qiniu.Auth.html)。

关键代码如下所示：

```php
  use Qiniu\Auth;

  $bucket = '<your_bucket>';
  $accessKey = '<your_access_key>';
  $secretKey = '<your_secret_key>';
  $auth = new Auth($accessKey, $secretKey);

  $upToken = $auth->uploadToken($bucket);

  echo $upToken;
```

可以看出，以上代码主要做了以下几步工作：

1. 使用你的 AccessKey 及 SecretKey 用于初始化`Auth`对象;
2. 调用初始化的对象`Auth`对象的方法`uploadToken`来生成上传token;

到这里为止，第一个示例就可以运行起来了。您可以查看该示例的[在线演示](http://demos.qiniu.com/demo/simpleuploader/)。您也可以直接获取和查看该示例的[源代码](https://github.com/rwifeng/qiniudocs/tree/master/demo/simpleuploader)。

### 小结

这个示例仅用于演示如何使用 PHP SDK 快速运行第一个云存储服务的程序。本事例尽可能简单的展现七牛的表单上传，因此请大家仅将本示例作为学习 PHP SDK 的一个步骤，而不是在产品环境中应用。详情请见：[表单上传](http://developer.qiniu.com/docs/v6/api/overview/up/form-upload.html)。

本文档接下来我们会介绍更接近于现实场景的一个示例。

<a id="android-demo"></a>
## 一个完整的移动应用

在本章我们将详细介绍如何使用 PHP SDK 与其他端配合构建一个类似于微信朋友圈的移动应用。

在这个移动应用里终端用户可以进行以下操作：

1. 使用预设的帐号密码登录（不支持注册新帐号）；
1. 查看自己和朋友分享的照片以及描述（不支持评论）；
1. 上传一张新图片，并添加一段描述；

[Demo截图](http://rwxf.qiniug.com/demo-screenshot.jpg)

### 总体架构

对于此类应用，我们推荐的总体架构如下图所示：

![arch-with-callback](http://developer.qiniu.com/docs/v6/api/overview/up/response/img/upload-with-callback.png "架构图")

对于此架构有以下关键要点：

1. AK/SK 只能在业务服务器端使用。如果将 AK 和 SK 同时保存或者传输到移动端会有严重的信息泄露风险；
1. AK/SK 是企业帐号信息，业务服务器需要有自己的帐号数据库，对应到每一个终端用户的帐号信息；
1. 由于不同的业务都需要存放一些跟文件相关的描述信息，且图片又属于不同的终端用户，因此需要维护文件管理表，以管理文件的描述信息和所有者关系。

基于以上这些关键要点，我们来设计和实现我们的 PHP 版本的业务服务器。

### 接口设计

我们来简单的设计我们的业务服务器接口。

#### 帐号验证

```http
POST /login.php HTTP/1.1
HOST: demos.qiniu.io
Accept: */*
Content-Type: application/x-www-form-urlencoded


uname=<username>&pwd=<password>
```

#### 获取文件列表

```http
POST /files.php HTTP/1.1
HOST: demos.qiniu.io
Accept: */*
Cookie: <cookie>
Content-Type: application/x-www-form-urlencoded

```

#### 获取上传授权

因为移动端并不知道 AK/SK 信息，客户端在需要上传文件时都需要向业务服务器发起一个获取上传授权的请求。

```http
POST /uptoken.php HTTP/1.1
HOST: demos.qiniu.io
Accept: */*
Cookie: <cookie>
Content-Type: application/x-www-form-urlencoded

```

#### 回调
  
移动端会直接上传文件到云存储服务，因此业务服务器不需要提供上传接口，但是需要提供一个供云存储服务在接收到文件后的回调接口。回调接口的响应内容会由云存储服务返回给移动端。

```http
POST /callback.php HTTP/1.1
HOST: demos.qiniu.io
Accept: */*
Cookie: <cookie>
Content-Type: application/x-www-form-urlencoded

uid=<uid>&fname=<file_name>&fkey=<file_key>&desc=<description>
```

#### 数据表

业务服务需要一个业务数据库的支撑以管理提到的帐号信息和文件信息。在本示例中我们选用 MySQL 来搭建业务数据库。

1. 帐号表

   字段名 | 字段类型 | 字段说明 |
   :--------- | :------------ | :------------ |
   id  | int | 唯一标识。 |
   uid | int | 唯一用户标识。 |
   uname | char(128) | 用户名，格式为邮箱如 admin@example.com 。 |
   password | char(128) | 加密后的密码。 |  
   status | int | 1: active; 0: disabled。 |

1. 文件信息表

   字段名 | 字段类型 | 字段说明 |
   :--------- | :------------ | :------------ |
   id  | int | 唯一标识。 |
   uid | int | 用户唯一标识。 |
   fname | char(512) | 文件的显示名 |
   key | char(512) | 文件对应到云存储服务中的唯一标识。 |
   createTime | time | 上传时间。 |
   description | char(2048) | 文件的描述内容 |

相应的 SQL 语句如下：

```sql
create database qspace;

CREATE TABLE users (
      uid            INT NOT NULL AUTO_INCREMENT,
      uname          VARCHAR(128) NOT NULL,
      password       VARCHAR(128) NOT NULL,
      status         INT, 
      PRIMARY KEY (uid)
);

CREATE TABLE files_info (
      id             INT NOT NULL AUTO_INCREMENT,
      uid            INT NOT NULL,
      fname          VARCHAR(512) NOT NULL,
      fkey           VARCHAR(512) NOT NULL,
      createTime     INT, 
      description    VARCHAR(1024), 
      PRIMARY KEY (id),
      FOREIGN KEY (uid) REFERENCES users(uid),
      UNIQUE INDEX (id)
);
```

### 服务实现

接下来我们一步步的实现以上定义的接口。这些实现主要考虑流程的完整性，因此在安全性上没有做充分的考量，请不要直接使用于产品环境中。

#### 配置文件

我们需要在一个地方存放全局的设置。本示例中我们直接用一个 config.php 文件包含若干全局变量的方式来解决这个问题。

```php
<?php
    class Config
    {
        const DB_NAME = 'qspace',
              DB_USER = 'root',
              DB_PWD = '****',
              DB_HOST = '<your_db_host>',
              ACCESS_KEY = '<your_access_key>',
              SECRET_KEY = '<your_secret_key>',
              BUCKET_NAME = '<your_bucket_name>';
    }
```

#### 帐号验证

帐号验证的逻辑非常简单，就是将请求中包含的用户信息与数据库表中的信息进行比对，因此不需要涉及 SDK 的功能。

```php
<?php
    require_once 'db.php';

    session_start();

    if(!isset($_POST['uname']) && !isset($_POST['pwd']))
    {
      http_response_code(401);
      $resp = array('status' => 'failed', 'msg' => 'please input username & password!');
      echo json_encode($resp);
      return;
    }

    $uname = $_POST['uname'];
    $_pwd = $_POST['pwd'];

    $salt = 'Qiniu' . $uname;
    $pwd = crypt($_pwd, $salt);

    $stmt = $DB->prepare('select * from users where uname = :name');
    $stmt->execute(array('name' => $uname));

    $user = $stmt->fetch();

    if ($user['password'] !== $pwd)
    {
      http_response_code(401);
      $resp = array('status' => 'failed', 'msg' => 'incorrect username or password!');
      echo json_encode($resp);
      return;
    }

    $_SESSION['uid'] = $user['uid'];
    $_SESSION['uname'] = $uname;

    $resp = array('status' => 'ok', 'uname' => $uname);
    echo json_encode($resp);
```

#### 获取文件列表

同帐号验证功能，获取文件列表也只是查询数据库，因此不多做解释。

```php
<?php
    require_once 'db.php';

    session_start();

    $uid = $_SESSION['uid'];
    if(!isset($uid))
    {
        header('location: login.php');
        return;
    }

    $stmt = $DB->prepare('select * from files_info where uid = :uid');
    $stmt->execute(array('uid' => $uid));

    $files = $stmt->fetchAll();

    echo json_encode($files);
```

#### 获取上传授权

客户端在需要上传文件时都需要先向业务服务器发起一个获取上传授权的请求。 SDK 中的 [`Qiniu\Auth`](http://developer.qiniu.com/php_docs/class-Qiniu.Auth.html) 类提供了 `uploadToken($bucket, ...)` 方法，可以非常便利的生成对应的上传授权。

```php
<?php
    require_once 'vendor/autoload.php';
    require_once 'db.php';
    require_once 'config.php';

    use Qiniu\Auth;

    session_start();
    $uid = $_SESSION['uid'];
    if(!isset($uid))
    {
      header('location: login.php');
      return;
    }

    $bucket = Config::BUCKET_NAME;
    $accessKey = Config::ACCESS_KEY;
    $secretKey = Config::SECRET_KEY;
    $auth = new Auth($accessKey, $secretKey);

    $policy = array(
      'callbackUrl' => 'http://172.30.251.210/callback.php',
      'callbackBody' => '{"fname":"$(fname)", "fkey":"$(key)", "desc":"$(x:desc)", "uid":' . $uid . '}'
      );

    $upToken = $auth->uploadToken($bucket, null, 3600, $policy);

    header('Access-Control-Allow-Origin:*');
    echo $upToken;
```

#### 回调

在收到回调时，通常表示一个文件已经成功上传。回调会包含该文件所对应的描述信息。因此业务服务器在收到回调后，需要将相应的文件信息写入到文件信息表中。

```php
<?php
    require_once 'db.php';

    $_body = file_get_contents('php://input');
    $body = json_decode($_body, true);

    $uid = $body['uid'];
    $fname = $body['fname'];
    $fkey = $body['fkey'];
    $desc = $body['desc'];

    $date = new DateTime();
    $ctime = $date->getTimestamp();

    $stmt = $DB->prepare('INSERT INTO files_info (uid, fname, fkey, createTime, description) VALUES (:uid, :fname, :fkey, :ctime, :desc);');
    $ok = $stmt->execute(array('uid' => $uid, 'fname' => $fname, 'fkey' => $fkey, 'ctime' => $ctime, 'desc' => $desc));

    header('Content-Type: application/json');
    if (!$ok)
    {
      $resp = $DB->errorInfo();
      http_response_code(500);
      echo json_encode($resp);
      return;
    }

    $resp = array('ret' => 'success');
    echo json_encode($resp);
```


### 服务监控

为了确认服务的正常运行，我们还实现了一个简单的监控页面以查看所有上传的图片。该页面假设`admin`的管理员才有权访问。
后端代码就是将用户上传的文件从数据库中列取出来。

```php
<?php
    require_once 'vendor/autoload.php';
    require_once 'db.php';

    if (!$_SESSION['logged'])
    {
      header('login.php');
    }

    $id = $_POST['id'];
    if ($id)
    {
      $stmt = $DB->prepare('delete from files_info where id = :id');
      $stmt->execute(array('id' => $id));
    }

    $stmt = $DB->prepare('select * from files_info');
    $stmt->execute();
    $files = $stmt->fetchAll();

    $smarty = new Smarty();
    $smarty->assign('files', $files);

    $smarty->display('file_mgr.tpl');
```

前端页面使用 [Bootstrap](http://getbootstrap.com/) 的控件实现，并使用 [Smarty](http://www.smarty.net/) 模板技术来循环生成最终页面：

```smarty
  {foreach from=$files item=file}
  <tr>
      <td><a href="{$file[uid]}">{$file["uid"]}</a></td>
      <td>{$file["fname"]}</td>
      <td>{$file["fkey"]}</td>
      <td>{$file["description"]}</td>
      <td>{$file["createTime"]}</td>
      <td><a class="del" href="" data-fid="{$file['id']}">删除</a></td>
  </tr>
  {/foreach}
```

### 移动端实现

本示例包含一个 Android 客户端的实现。因为本文档的重心是结合例子讲解 PHP SDK 的使用，因此这里就不详细讲解如何实现 Android 客户端了。您可以下载和安装移动客户端的[安装包](http://rwxf.qiniudn.com/app-release.apk)，或查看移动客户端的[源代码](https://github.com/simon-liubin/android-demo)。

### 小结

您可以在 Android 手机或者模拟器上安装和运行本示例的移动端应用，上传一张图片，并查看图片列表和描述。这个示例的重点在于讲解一个推荐的产品架构，以及各个子系统是如何协同工作。

接下来我们会再围绕几个更多的示例遍历七牛云所提供的其他强大的功能。接下来的示例都不会再像这个示例一样提供一个完整的移动互联网产品架构，而是把重点放在功能介绍上。您可以举一反三，快速的在本章介绍的示例上进行修改和加强，就可以快速开发出各种有趣的移动互联网应用。  移动端[安装包](http://rwxf.qiniudn.com/app-release.apk)，或查看移动客户端的[源代码](https://github.com/simon-liubin/android-demo)， [admin管理界面](http://demos.qiniu.com/demo/qspace/)。

<a id="image-proc"></a>
## 图片处理

图片内容是几乎所有互联网应用都需要管理的数据类型。我们的服务提供了比较强大的数据处理功能。本章我们讲围绕一个功能相对完备的示例来讲解图片处理的主要用法，并顺便介绍 PHP SDK 的资源管理功能。

这个示例用户可以上传图片，选择图片处理模式，输入相应的处理参数，然后可以查看处理的结果。用户还可以点选不同的图片以显示图片的鉴黄信息和鉴别广告的信息， 以及获取图片的基本信息，平均色，exif信息等。

下面我们分步来实现这个示例。前端实现使用了 [Bootstrap](http://getbootstrap.com/)和我们的[js-sdk](https://github.com/qiniupd/qiniu-js-sdk), 后端使用phpsdk生成上传的token。

###  生成上传token

和之前的代码一样， 首先安装我们的phpsdk然后引入相应文件并调用接口生成上传token。
具体php代码：

```php
    require_once 'vendor/autoload.php';
    require_once 'config.php';

    use Qiniu\Auth;

    $bucket = Config::BUCKET_NAME;
    $accessKey = Config::ACCESS_KEY;
    $secretKey = Config::SECRET_KEY;

    $auth = new Auth($accessKey, $secretKey);
    $upToken = $auth->uploadToken($bucket);

    $ret = array('uptoken' => $upToken);

    echo json_encode($ret);
```


上传部分相关的html代码：

```html
<div id="container">
     <button id="pickfiles" class="btn btn-primary btn-lg btn-block" type="submit">上传图片</button>
</div>                
```

上传对应的调用jssdk相关代码如下所示：

```js
    var uploader = Qiniu.uploader({
        runtimes: 'html5,flash,html4', //上传模式,依次退化
        browse_button: 'pickfiles', //上传选择的点选按钮，**必需**
        uptoken_url: 'uptoken.php', //Ajax请求upToken的Url，**强烈建议设置**（服务端提供）
        domain: 'http://rwxf.qiniudn.com/', //bucket 域名，下载资源时用到，**必需**
        container: 'container', //上传区域DOM ID，默认是browser_button的父元素，
        max_file_size: '100mb', //最大文件体积限制
        flash_swf_url: 'plupload/Moxie.swf', //引入flash,相对路径
        max_retries: 3, //上传失败最大重试次数
        dragdrop: true, //开启可拖曳上传
        drop_element: 'container', //拖曳上传区域元素的ID，拖曳文件或文件夹后可触发上传
        chunk_size: '4mb', //分块上传时，每片的体积
        auto_start: true, //选择文件后自动上传，若关闭需要自己绑定事件触发上传
        init: {
            'UploadProgress': function(up, file) {
                $('#pickfiles').prop('disabled', true).html('图片上传中...');
            },
            'FileUploaded': function(up, file, info) {

                $('#pickfiles').prop('disabled', false).html('上传图片');
                var res = JSON.parse(info);
                imgUrl = up.getOption('domain') + res.key;
                refresh(imgUrl);
            },
            'Error': function(up, err, errTip) {
                $('#pickfiles').prop('disabled', false).html('上传图片');
            }
        }
    });
```

### 图片处理

图片处理的过程非常简单，就是将我们的图片处理 `fop` 以及对应的参数拼接在图片地址后面即可。 基本不需要用到 SDK 的功能，当然你也可以使用我们的jssdk进行图片地址和处理参数的拼接。
比如一个图片

```
http://qiniuphotos.qiniudn.com/gogopher.jpg
```
![原图](http://qiniuphotos.qiniudn.com/gogopher.jpg)

```
现在我们对这个图片进行200x200的等比缩放，然后再进行居中剪裁: 可以使用imageView2 的mode 1
http://qiniuphotos.qiniudn.com/gogopher.jpg?/1/w/<Width>/h/<Height>

最终得到的处理后的图片为：
http://qiniuphotos.qiniudn.com/gogopher.jpg?imageView2/1/w/200/h/200
```

![](http://qiniuphotos.qiniudn.com/gogopher.jpg?imageView2/1/w/200/h/200)


更多处理处理规格可以参考我们的[图片处理文档](http://developer.qiniu.com/docs/v6/api/reference/fop/image/imageview2.html)

### 图片信息
* 图片基本信息
只需要在图片外链后面拼接上  `?imageInfo`

http://qiniuphotos.qiniudn.com/gogopher.jpg?imageInfo

```json
{
  format: "jpeg",
  width: 640,
  height: 427,
  colorModel: "ycbcr",
  orientation: "Top-left"
}
```

* 图片平均色
只需要在图片的外链后面拼接上  `?imageAve`

http://qiniuphotos.qiniudn.com/gogopher.jpg?imageAve

```json
{
  RGB: "0x85694d"
}
```

* 图片Exif信息
只需在图片的外链后面拼接上 `?exif`

http://qiniuphotos.qiniudn.com/gogopher.jpg?exif

```json
{
    ApertureValue: {
        val: "5.00 EV (f/5.7)",
        type: 5
    },
    ColorSpace: {
        val: "sRGB",
        type: 3
    },
    ComponentsConfiguration: {
        val: "- - - -",
        type: 7
    },
    ...
}

```

### 高级UFOP功能

除去我们官方提供的强大图片处理功能， 也可以体验我们 UFOP 平台提供的强大第三方数据处理服务。在本例中我们就来尝试一下[色情图片识别功能（ NROP）](http://developer.qiniu.com/docs/v6/api/reference/fop/third-party/nrop.html) 。

要使用 NROP 功能，用户需要先在管理平台上的[应用市场](https://portal.qiniu.com/service/market)开启本功能。开启后使用方式与一般的数据处理指令完全一致（比如获取 EXIF 信息的接口），仅需要使用带 nrop 参数的 GET 请求即可，返回的 HTTP 响应内容为一个包含鉴定结果的 JSON 字符串。示例代码如下所示：

http://qiniuphotos.qiniudn.com/gogopher.jpg?nrop

```json
{
    statistic: [
        0,
        0,
        1
    ],
    reviewCount: 0,
    fileList: [{
        rate: 0.9946920275688171,        // 介于0-1间的浮点数，表示该图像被识别为某个分类的概率值，概率越高、机器越肯定
        label: 2,                        // 0：色情； 1：性感； 2：正常
        name: "739a77baf4ff2d5eae5fe56602fc0cbe/gogopher.jpg",
        review: false                    //是否需要人工复审该图片，鉴黄服务是否对结果确定(true：不确定，false：确定)
    }],
    nonce: "0.5508577267173678",
    timestamp: 1437903830,
    code: 0,
    message: "success"
}

```


我们提供的完整示例已经包含了对 NROP 的调用，以及对广告的鉴定，您可以体验一下效果。

### 小结

这个示例结束后，相信你已经比较了解我们的平台是如何支持图片内容的编辑，基本上这些动作都只是一个简单的 GET 请求即可完成，甚至都不需要依赖于 SDK。您可以查看本示例的[在线演示](http://demos.qiniu.com/demo/qimage/index.html)，或查看和下载本示例的[完整源代码](https://github.com/rwifeng/qiniudocs/tree/master/demo/qimage)。

<a id="av-proc"></a>
## 音视频处理

在介绍完图片的简单管理方法后，我们接下来用一个示例详细讲解七牛云对音视频和流媒体格式的强大支持。该示例的目标为完成以下动作：

1. 支持断点续传的大文件上传，因为视频文件通常比较大，难以用单次 POST 请求完成上传；
1. 支持上传后异步执行的自动转码动作，生成若干不同规格的目标视频，分别适合在手机和电脑上播放；
1. 将文件转换为 HLS 格式存放，以支持边下载边播放的效果；
1. 为视频打上一个图片水印;
1. 统一从每一个视频文件抽取一个固定时间点的画面作为预览图片;
1. 在网页播放器中播放生成的视频文件;

我们选用 [plupload](http://www.plupload.com/) 作为我们的上传控件，并使用 [videojs](http://www.videojs.com/) 作为我们的网页播放器。其他采用的技术与之前的示例一致，主要是 Bootstrap 和 Smarty 。因为文件为客户端直传，因此我们也需要提供一个回调服务，以便于接收上传和转码这些异步任务的完成事件。为了简单起见，该示例就不再实现一个独立的业务数据库了，直接从目标存储空间获取文件信息。

### 大文件上传

因为大文件上传必须在浏览器端进行，因此我们就不演示如何用 PHP SDK 做断点续上传了。网页端的大文件上传我们可以用定制版本的 plupload 来支持。
具体可以查看我们的[jssdk](https://github.com/qiniu/js-sdk)

### 上传后自动转码

我们可以通过设置上传策略来通知云存储服务在上传完成后自动发起一个异步的任务。上传策略在调用上传接口时作为参数传入。
这里的转码过程需要支持转为 HLS 格式，并且在转码后打上视频水印。具体生成上传策略的代码为：

```php
    $bucket = Config::BUCKET_NAME;
    $auth = new Auth(Config::AK, Config::SK);

    $wmImg = Qiniu\base64_urlSafeEncode('http://rwxf.qiniudn.com/logo-s.png');
    $pfopOps = "avthumb/m3u8/wmImage/$wmImg";
    $policy = array(
        'persistentOps' => $pfopOps,
        'persistentNotifyUrl' => 'http://<your_notify_url>',
    );

    $upToken = $auth->uploadToken($bucket, null, 3600, $policy);

    echo json_encode(array('uptoken' => $upToken));

```


### 抽取视频截图

  我们可以从上传视频中截取固定时间点得帧，以作为这些视频的封面图片。视频截图如下所示：

```
    http://<your_uploaded_video>?vframe/jpg/offset/5
```

### 视频浏览与播放

在完成以上工作后，接下来的工作就简单了。我们可以生成一个列表网页，每个列表项都是一张视频截图，点击视频截图会弹出一个播放面板。因为类似的前端代码已经在本文档展示过，这里就不再多展示一次。

### 小结

视频的示例就到这里完成了。您可以直接查看本示例的[在线演示](http://demos.qiniu.com/demo/qav/index.php)，或下载和查看本示例的[完整源代码](https://github.com/rwifeng/qiniudocs/tree/master/demo/qav)。

需要注意的是， 我们这边使用了videojs的开源播放器， 世面上还有其他比较优秀的播放器，具体你可以参考[播放器推荐](http://kb.qiniu.com/5a9mzj6n) 。

另外，对于视频内容，CDN 的选择会是一个影响视频播放是否能够足够流畅的关键因素。我们的[多 CDN 管理平台](https://fusion.qiniu.com/#/)会给列出的可用 CDN 线路标注是否适用于视频加速，请合理选择。

<a id="faq"></a>
# FAQ
- - -


<a id="related-resources"></a>
# 相关资源
- - -

到这里为止，我们就把七牛所提供的关键功能都浏览了一遍。如果您有一些关于本文档或我们产品的建议和想法，欢迎到我们的[技术社区](http://segmentfault.com/qiniu/)参与讨论。

* [技术论坛](http://segmentfault.com/qiniu) - 在这里你可以和其他开发者愉快的讨论如何更好的使用七牛云服务
* [提交工单](http://support.qiniu.com/hc/request/guest/) - 如果你的问题不适合在论坛讨论或得不到回答，你可以提交一个工单，技术支持人员会尽快回复
* [博客](http://blog.qiniu.com) - 这里会持续发布市场活动和技术分享文章
* [微博](http://weibo.com/qiniutek)
* [常见问题FAQ](http://developer.qiniu.com/docs/v6/faq/faq.html)


其他对于 PHP 开发者有用的资源：

* 示例库：[https://github.com/qiniudemo/](https://github.com/qiniudemo/)
* 第三方示例库：[https://github.com/jemygraw/](https://github.com/jemygraw/qiniu-lab-php)


---
title: ✍PHP的composer相关用法说明
date: 2023-10-06
tags: PHP
---

本篇文章记录php的composer组件包一些用法！

## 引用本地未发布的包

要引用本地未发布的包,只需要在composer.json中`repositories`仓库为为本地项目地址即可，如下所示：

```json
{
    "repositories": [
        {
            "type": "path",
            "url": "/path/to/local/package"
        }
    ],
    "require": {
        "your-package": "dev-master"
    }
}
```
<!--more-->>
然后执行`composer install` 或 `composer require <your-package@dev-master>`

## 排除特定文件或目录不被自动加载

在composer.json中配置`exclude-from-classmap`即可，如下所示：

```json
{
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        },
        "exclude-from-classmap": [
            "src/Helper/Deprecated.php",
            "src/UnusedDirectory/"
        ]
    }
}
```

## composer 版本号约定规则

Composer中，版本号遵循语义化版本（Semantic Versioning）的约定规则。语义化版本由三个数字组成：MAJOR.MINOR.PATCH。以下是每个部分的含义：

1. MAJOR：主要版本号。当您进行不兼容的API更改时，应该增加主要版本号。这表示可能会引入破坏性更改，可能需要重新编写依赖于该软件包的代码。
2. MINOR：次要版本号。当您添加向后兼容的功能时，应该增加次要版本号。这表示没有破坏性更改，只是增加了新功能。
3. PATCH：补丁版本号。当您进行向后兼容的错误修复或其他维护性更改时，应该增加补丁版本号。这表示没有破坏性更改或新功能，只是修复了一些错误。

此外，版本号还可以包含预发布标识符和构建元数据。预发布标识符用于标识非稳定版本，例如`alpha`、`beta`、`rc`等。构建元数据用于标识特定构建的额外信息。

**以下是一些版本号示例：**

1. 1.0.0：稳定的版本1.0.0。
2. 2.1.3：稳定的版本2.1.3。
3. 3.0.0-alpha3：预发布版本3.0.0的第3个alpha版本。
4. 1.2.0-beta2+20130313144700：预发布版本1.2.0的第2个beta版本，并带有构建元数据。
5. ^1.0：允许任何1.x版本，但不包括2.0及更高版本。
6. ~1.2.3：允许任何1.2.x版本，但不包括1.3及更高版本。
7. \>=1.0,<2.0：允许1.x版本，但不包括2.0及更高版本。
8. 6.1|7.1 允许6.1或7.1 版本
9. 1.1.* 1.1.X 所有版本

## composer 新增自定义脚本

在composer.json中配置`scripts`即可，如下所示：

```json
{
    "scripts": {
        "scripts-name": "echo 'This is a post-installation script.'"
    }
}
```

`scripts-name` 是脚本命令名称,执行命令如：`composer post-install-cmd` 即执行`echo 'This is a post-installation script.'`shell命令。

除了自定义脚本命令之外，composer包含`post-install-cmd` 和 `pre-update-cmd` 等相关内置命令。

1. `post-install-cmd` 用于在安装依赖后执行自定义操作。当运行composer install命令时，Composer会在安装依赖包之后自动运行post-install-cmd脚本。
2. `pre-update-cmd` 用于在更新依赖包之前执行自定义操作。当运行composer update命令时，Composer会在更新依赖包之前自动运行pre-update-cmd脚本。

更多的composer 一些使用说明，请移步[composer中文文档](https://docs.phpcomposer.com/)。

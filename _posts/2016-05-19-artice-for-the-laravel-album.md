---
layout: post
title: 快学Laravel系列
date: 2016-09-10 15:30:22
category: "Laravel"
---

### 类库管理神器Composer安装和配置laravel5.2

#### composer 下载安装

- 英文网站：[https://getcomposer.org/](https://getcomposer.org/download/)

- 中文镜像：[http://www.phpcomposer.com/](http://www.phpcomposer.com/)

安装 

- [win 下安装](http://bbs.houdunwang.com/thread-100920-1-1.html)
- [mac 下安装](http://bbs.houdunwang.com/thread-100921-1-1.html)

#### composer 安装 Laravel

1.直接下载安装（需要翻墙）

- 创建一个名为 laravel 的 Laravel 项目
`composer create-project laravel/laravel --prefer-dist`

- 创建一个名为 blog 的 Laravel 项目
`composer create-project laravel/laravel blog --prefer-dist`

2.使用 composer 镜像安装（不要翻墙）

启用镜像服务的方式有两种:

- **系统全局配置:** 将全局配置信息添加到 Composer 的全局配置文件 config.json 中
- **单个项目配置:** 配置信息添加到项目目录 composer.josn 中

例1：修改 composer 的全局配置文件（推荐方式）

打开命令行窗口（windows用户）或控制台（Linux、Mac 用户）并执行如下命令：

`composer config -g repo.packagist composer https://packagist.phpcomposer.com`

例2：修改当前项目的 composer.json 配置文件：

 **空目录状态下需要手动创建配置文件,并在当前文件中写入 json 的包含符号 {}**

打开命令行窗口（windows用户）或控制台（Linux、Mac 用户），进入你的项目的根目录（也就是composer.json 文件所在目录），执行如下命令：

`composer config repo.packagist composer https://packagist.phpcomposer.com`

上述命令将会在当前项目中的 composer.json文件的末尾自动添加镜像的配置信息（你也可以自己手工添加）：

```
"repositories": {
   "packagist": { 
    "type": "composer", 
    "url": "https://packagist.phpcomposer.com"
   }
}
```

以 laravel 项目的 composer.json 配置文件为例，执行上述命令后如下所示（注意最后几行）：

```
{
    "name": "laravel/laravel",
    "description": "The Laravel Framework.",
    "keywords": [
        "framework",
        "laravel"
    ],
    "license": "MIT",
    "type": "project",
    "require": {
        "php": ">=5.5.9",
        "laravel/framework": "5.2.*"
    },
    "require-dev": {
        "fzaninotto/faker": "~1.4",
        "mockery/mockery": "0.9.*",
        "phpunit/phpunit": "~4.0",
        "symfony/css-selector": "2.8.*|3.0.*",
        "symfony/dom-crawler": "2.8.*|3.0.*"
    },
    "autoload": {
        "classmap": [
            "database"
        ],
        "psr-4": {
            "App\\": "app/"
        }
    },
    "autoload-dev": {
        "classmap": [
            "tests/TestCase.php"
        ]
    },
    "scripts": {
        "post-root-package-install": [
            "php -r \"copy('.env.example', '.env');\""
        ],
        "post-create-project-cmd": [
            "php artisan key:generate"
        ],
        "post-install-cmd": [
            "php artisan clear-compiled",
            "php artisan optimize"
        ],
        "pre-update-cmd": [
            "php artisan clear-compiled"
        ],
        "post-update-cmd": [
            "php artisan optimize"
        ]
    },
    "config": {
        "preferred-install": "dist"
    },
    "repositories": {
        "packagist": {
            "type": "composer",
            "url": "https://packagist.phpcomposer.com"
        }
    }
}
```
OK，一切搞定！试一下 composer install 来体验飞一般的速度吧！

### 4.Laravel5.2一键安装包安装及初始化配置

#### Laravel 下载安装方式

- 一键安装包下载  `http://www.golaravel.com/download/`
- GitHub 下载
 + [GitHub Clone](https://github.com/laravel/laravel)
 + 直接下载zip包
 + SourceTree 克隆下载

#### Laravel 初始化配置

1. wamp 版本需求 ( PHP 版本 >= 5.5.9 | Wamp2.5 )
2. Apache 配置 httpd.conf 开启 rewrite 和 vhost
3. 开启 PHP 扩展 php.ini
 - extension=php_openssl.dll
 - extension=php_mbstring.dll
 - extension=php_pdo_mysql.dll  
#### 运行Laravel启动欢迎页
1. 使用下载安装方法安装laravel5.2，需要重新生成key `cli 中执行 php artisan key:generate
`
2. 修改默认首页、伪静态配置文件.htaccess
 > 需要注意的是安装后,可访问目录指向 Public 或项目根目录的问题,如果是项目根目录,可重命名 server.php 并提取 Public 目录中的**伪静态配置文件**至上一层目录 
 
 >注：独立服务器，有修改入口文件目录权限或者子目录绑定域名的情况下可直接在分布式虚拟配置中直接入口指向 public 目录

#### 参考文档
- [中文文档](http://laravelacademy.org/laravel-docs-5_2) 
- [英文文档](https://laravel.com/docs/5.2)
> 文档使用原则: 时间是检验真理的唯一标准

### Laravel5.2目录结构及composer.json文件解析

| Folder  or file | describe  |
|:----|----|
| ｜–　app |  包含Controller、Model、路由等在内的应用目录，大部分业务将在该目录下进行|
| ｜　　｜–　Console | 命令行程序目录 |
| ｜　　｜　　｜–　Commands | 包含了用于命令行执行的类，可在该目录下自定义类 |
| ｜　　｜　　｜–　Kernel.php | 命令调用内核文件，包含commands变量(命令清单，自定义的命令需加入到这里和schedule方法(用于任务调度，即定时任务) |
| ｜　　｜–　Events | 事件目录 |
| ｜　　｜–　Exceptions | 包含了自定义错误和异常处理类 |
| ｜　　｜–　**Http** | HTTP传输层相关的类目录 |
| ｜　　｜　　｜–　Controllers | 控制器目录 |
| ｜　　｜　　｜–　Middleware | 中间件目录 |
| ｜　　｜　　｜–　Requests | 请求类目录 |
| ｜　　｜　　｜–　Kernel.php | 包含  http 中间件和路由中间件的内核文件 |
| ｜　　｜　　｜–　routes.php | 强大的路由 |
| ｜　　｜–　Jobs | 该目录下包含队列的任务类 |
| ｜　　｜–　Listeners| 监听器目录 |
| ｜　　｜–　Providers| 服务提供者目录 |
| ｜　　｜–　User.php | 自带的模型实例,我们吸纳进的 Model 默认也会在该目录中 |
| ｜–　bootstrap | 框架启动载入目录,仅和前段框架重名而已 |
| ｜　　｜–　app.php | 创建框架应用实例 |
| ｜　　｜–　autoload.php | 自动加载 |
| ｜　　｜–　cache | 存放框架启动缓存，web服务器需要有该目录的写入权限 |
| ｜–　config | 各种配置文件的目录 |
| ｜　　｜–　app.php | 系统级配置文件 |
| ｜　　｜–　auth.php | 用户身份认证配置文件,指定好 table 和 model 就可以很方便地用身份认证功能 |
| ｜　　｜–　broadcasting.php | 事件广播配置文件 |
| ｜　　｜–　cache.php | 缓存配置文件 |
| ｜　　｜–　compile.php | 编译额外文件和类需要的配置文件，一般用户很少用到 |
| ｜　　｜–　database.php | 数据库配置文件 |
| ｜　　｜–　filesystems.php | 文件系统配置文件，这里可以配置云存储参数 |
| ｜　　｜–　mail.php | 电子邮件配置文件 |
| ｜　　｜–　queue.php| 消息队列配置文件 |
| ｜　　｜–　services.php | 可存放第三方服务的配置信息 |
| ｜　　｜–　session.php | 配置session的存储方式、生命周期等信息 |
| ｜　　｜–　view.php | 模板文件配置文件，包含模板目录和编译目录等 |
| ｜–　database | 数据库相关目录 |
| ｜　　｜–　factories | 5.1以上版本的新特性，工厂类目录，也是用于数据填充 |
| ｜　　｜　　｜–　ModelFactory.php | 在该文件可定义不同 Model 所需填充的数据类型,项目协作中使用代码方式给数据库填充假数据 |
| ｜　　｜–　migrations | 存储数据库迁移文件|
| ｜　　｜–　seeds | 存放数据填充类的目录|
| ｜　　｜　　｜–　DatabaseSeeder.php | 执行 php artisan db:seed 命令将会调用该类的run方法。该方法可调用执行该目录下其他Seeder类，也可调用factories方法生成 ModelFactory 里定义的数据模型|
| ｜–　public | 网站入口，应当将ip或域名指向该目录而不是根目录。可供外部访问的css、js和图片等资源皆放置于此 |
| ｜　　｜–　index.php | 入口文件 |
| ｜　　｜–　.htaccess | Apache 服务器用该文件重写URL |
| ｜　　｜–　web.config| IIS服务器用该文件重写URL |
| ｜–　resources| 资源文件目录|
| ｜　　｜–　assets| 可存放包含 LESS 、 SASS 、 CoffeeScript 在内的原始资源文件|
| ｜　　｜–　lang | 本地化文件目录,根据来访者操作系统,返回中/英文 |
| ｜　　｜–　views | 视图文件存放目录 |
| ｜–　storage | 存储目录。web服务器需要有该目录及所有子目录的写入权限 |
| ｜　　｜–　app | 可用于存储应用程序所需的一些文件 |
| ｜　　｜–　framework | 该目录下包括缓存、 sessions 和编译后的视图文件 |
| ｜　　｜–　logs | 日志目录 |
| ｜–　tests | 测试目录 |
| ｜–　vender | 该目录下包含Laravel源代码和第三方依赖包 |
| ｜–　.env | 环境配置文件。 config 目录下的配置文件会使用该文件里面的参数，不同生产环境使用不同的.env文件即可|
| ｜–　artisan | 强大的命令行接口，你可以在 app/Console/Commands 下编写自定义命令 |
| ｜–　composer.json | 存放依赖关系的文件 |
| ｜–　composer.lcok | 锁文件，存放安装时依赖包的真实版本 |
| ｜–　gulpfile.js | gulp（一种前端构建工具）配置文件 |
| ｜–　package.json | gulp配置文件 |
| ｜–　phpspec.yml | phpspec（一种PHP测试框架）配置文件 |
| ｜–　phpunit.xml | phpunit（一种PHP测试框架）配置文件 |
| ｜–　server.php | PHP内置的Web服务器将把这个文件作为入口。以public/index.php为入口的可以忽略掉该文件 |


### Laravel5.2 HTTP
#### 路由基础-路由参数
- 必选参数
- 可选参数
- 参数约束
 - 正则约束
#### 控制器
>可理解控制器为:数据和视图之间的桥梁
控制器创建方法:
1. 手动创建 
2. Artisan  方法创建 `php artisan make:controller HomeController`会自动命名,依赖

扩展:一般项目中控制器分文件夹操作,对路由指向,命名空间,继承关系进行更新

#### 高级路由 
路由命名:`Route::get('test','IndexController@index')->name('test)`

路由群组:
```
Route::group(['prefix' => 'admin'
,'namespace'=>'Admin'], function () {
    Route::get('index','IndexController@index');
    Route::get('login','IndexController@login');
    Route::resource('article','ArticleController');
});
```

#### 中间件 Middleware
>使用 artisan 创建中间件 `php artisan make:middleware Admin`
>5.1和 5.5　Laravel 版本的 web csrf 中间件使用有不一样之处
- 路由分组
- 路由前缀 prefix namespace
- 子域名路由
- 中间件

#### 视图
数据传递

- with 单个参数传递
- 传参 `$data` 
- compact
 
### Blade 模板引擎

#### 基本用法

- {{ $name }}

- @{{ $name }}

- {{ $name or 'Default'}}

- {{ isset($name)?$name:'default'}}

- {!! $script !!}

原创文章转载请注明出处：[快学Laravel系列](https://unclethree.github.io/laravel/2016/09/10/artice-for-the-laravel-album.html)


 











 



  





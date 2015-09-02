# Laravel规范

*9birds 开发 Laravel Application 时的 Style Guide。*

# 建设中...

<a name="table-of-contents"></a>
## 目录

  1. [目录结构](#目录结构)
    - [Application核心](#Application核心)
    - [其他](#其他)
  1. [第三方组件](#第三方组件)
    - [require](#require)
    - [require-dev](#require-dev)
  1. [Coding Style](#Coding Style)
    - [代码相关](#代码相关)
    - [命名相关](#命名相关)
      - [数据库表和字段](#数据库表和字段)
      - [类和方法](#类和方法)
      - [变量](#变量)


## 目录结构
### Application核心
```
app
├── Console - 自定义 Command
│   └── Commands
├── Events - 事件（对应 Listeners）
├── Exceptions - 自定义 Exceptions
├── Http
│   ├── Controllers
│   ├── Middleware
│   └── Requests
├── Jobs - 任务队列
├── Listeners - 监听器（对应 Events 及 Jobs（Queue））
├── Models - 所有 Eloquent Model
├── Repositories - 所有封裝的 业务Models
└── Providers - 框架的 Service Provider
```
### 其他
```
├── bootstrap - 框架启动的核心
├── config - 框架的設定檔案
├── database - 数据库相关
│   ├── factories - 数据库工厂
│   ├── migration - 数据库迁移
│   └── seeds - 数据填充
├── public - 存放公有的 Assets
│   ├── imgs - 图片
│   ├── css - Css
│   ├── fonts - 字体
│   └── js - JavaScript
├── resources - 资源
│   ├── assets - 需要预编译的Assets
│   │   ├── sass
│   │   ├── css
│   │   └── js
│   ├── lang - 多国语言
│   └── views - 视图
├── storage - 框架的Cache等
├── tests - Unit Test
└── vendor - 所有的框架核心及 Package
```
## 第三方组件
初始化 Project 时请添加以下 Package，并查阅相关文档加上相应的 Service Provider 及 Facade
### require
- `barryvdh/laravel-debugbar`
  - https://github.com/barryvdh/laravel-debugbar
  - 用于查看 booting time, query statement 或显示变量

### require-dev
- `barryvdh/laravel-ide-helper`
  - https://github.com/barryvdh/laravel-ide-helper
  - 输出 Class结构的文档，帮助 IDE 做 autocomplate
  - 在 composer.json 的 scripts 加入以下代码，即可使用
    composer run-script ide-helper 创建文档

```
...
"scripts": {
    ...
    "ide-helper": [
        "php artisan clear-compiled",
        "php artisan ide-helper:generate",
        "php artisan ide-helper:meta",
        "php artisan ide-helper:models --write",
        "php artisan optimize"
    ]
},
...
```
评估中

- https://github.com/prettus/l5-repository
- https://github.com/Bosnadev/Repositories

## Coding Style
### 代码相关
- 所有代码請參照 PSR-1 及 PSR-2 规范，可使用 phpcs 等工具协助检查
  - PSR-1 中文 http://blog.mosil.biz/2012/08/psr-1-basic-coding-standard/
  - PSR-2 英文 http://www.php-fig.org/psr/psr-1/
  - PSR-2 中文 http://blog.mosil.biz/2012/09/psr-2-basic-coding-standard/
  - PSR-2 英文 http://www.php-fig.org/psr/psr-2/

- 遵照 ARCA pattern
  - ARCA http://blog.turn.tw/?p=2290
  - Repository http://blog.turn.tw/?p=2521
  - Presenter http://blog.turn.tw/?p=2511

- 在 Class 中避免使用 Facade，请改用 DI 替代

```php
//bad
use Request;
...
public function store()
{
    $data = Request::all()
}

//good
use Illuminate\Http\Request;
...
public function store(Request $request)
{
    $data = $request->all()
}
```

### 命名相关
#### 数据库表和字段
- 資料表名稱請使用**複數英文**並使用**蛇底式命名法（snake_case）**

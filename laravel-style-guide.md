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
├── Repositories - 所有封装的 业务Models
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
- 数据库表名称请使用 **复数英文** 并使用 **蛇底式命名法（snake_case）**

```php
// bad
Schema::create('User', ...
Schema::create('user', ...

// good
Schema::create('users', ...
```

- 多对多（或同性质）的 pivot table 请命名为`{单数表A}_{单数表B}`，并按照 **英文字母順序排序**

```php
// bad
Schema::create('articles_tags', ...
Schema::create('article_tag_mapping', ...
Schema::create('tag_article', ...

// good
Scheam::create('article_tag', ...
```

- 数据库字段请使用 **蛇底式命名法（snake_case）** 且一律 **不加前綴字**

```php
//bad
$table->string('Name');
$table->string('u_name');

// good
$table->string('name');
```

- Primary Key 一律为 `id`

```php
// bad
$table->increment('uid');
$table->increment('pId');
$table->increment('p_id');

// good
$table->increment('id');
```

- Foreign Key 一律为`{单数表名称}_id`

```php
// bad
$table->integer('UserId');
$table->integer('userId');

// good
$table->integer('user_id');
```

#### 类和方法
- 所有类名称请使用 **帕斯卡命名法（PascalCase）**

```php
// bad
class Articles_Repository {...}
class articlesRepository {...}

// good
class ArticlesRepository {...}
```

- 所有的 Resources Controller 名称为 **复数**

```php
// bad
class ArticleController extends Controller {...}

// good
class ArticlesController extends Controller {...}
```

- 所有 Eloquent Model 名称为 **单数**

```php
// bad
class Users extends Model {...}

// good
class User extends Model {...}
```

- 所有方法名称请使用 **驼峰式命名法（camelCase）**

```php
// bad
public function CreateArticle {...}
public function create_article {...}

//good
public function createArticle {...}
```

#### 变量
- 所有 PHP 变量名称请使用 **驼峰式命名法（camelCase）**

```php
// bad
$request_data;
$Request_Data;

//good
$requestData;
```

- 所有 HTTP 传递的参数名称请使用 **蛇底式命名法（snake_case）**

```php
// bad
{!! Form::text('addressSwitch') !!}

//good
{!! Form::text('address_switch') !!}
```

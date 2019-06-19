## PHP 框架篇

### <div id="生命周期"> 生命周期</div> 
- 单一入口
- Application(应用实例/服务容器)
- Kernel(内核)
- Request(请求)
- Response(响应)

生命周期

- nginx 把 http 请求转发给 public/index.php
- 创建 Application 对象
- 往 Application 里面注册 App\Http\Kernel::class 对象（Kernel）
- 使用 Application 创建 Kernel 对象
- 创建 Request 对象
- 调用 Kernel 的 handle 方法处理 Request 请求，并返回 Response 对象
- 使用 Response 的 send 方法输出结果
- 调用 Kernel 的 terminate 方法做相关清理的操作

### <div id="路由"> 路由(Web,Api)</div> 
laravel中路由在web.php和api.php中进行定义：如：

Route::get('user/{name}', function ($name) {
    // 只允许一个以上大小写字母
})->where('name', '[A-Za-z]+');

Route::get('user/{id}', function ($id) {
    // 只允许一个以上数字
})->where('id', '[0-9]+');

Route::get('user/{id}/{name}', function ($id, $name) {
    // 同时限定两个参数
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);

两个文件的区别是：在web.php中定义的路由默认不允许跨域(使用csrf)，而api.php中的允许跨域请求。

当然也可以在 VerifyCsrfToken 中间件中（文件位置：app/Http/Middleware/VerifyCsrfToken.php）将要排除的 URL 添加到 $except 属性数组中。即为此url不使用该中间件。

### <div id="中间件"> 中间件</div> 

中间件提供了一种方便的机制过滤进入应用程序的 HTTP 请求。例如，Laravel 包含一个中间件，验证您的应用程序的用户身份验证。如果用户未被认证，中间件会将用户重定向到登录界面。然而，如果用户通过身份验证，中间件将进一步允许请求到应用程序中。

当然，除了身份认证以外，还可以编写另外的中间件来执行各种任务。例如：CORS 中间件可以负责为所有离开应用的响应添加合适的头部信息；日志中间件可以记录所有传入应用的请求。

Laravel 自带了一些中间件，包括身份验证、CSRF 保护等。所有这些中间件都位于 app/Http/Middleware 目录。

通过运行 make:middleware Artisan 命令来创建新的中间件：

	php artisan make:middleware CheckAge

>该命令会在 app/Http/Middleware 目录下创建一个新的 CheckAge 类，在这个中间件中，我们仅允许 age 参数大于 200 的请求对此路由进行访问，否则，将重定向到 home

	<?php
	
	namespace App\Http\Middleware;
	
	use Closure;
	
	class CheckAge
	{
	    /**
	     * Handle an incoming request.
	     *
	     * @param  \Illuminate\Http\Request  $request
	     * @param  \Closure  $next
	     * @return mixed
	     */
	    public function handle($request, Closure $next)
	    {
	        if ($request->age <= 200) {
	            return redirect('home');
	        }
	
	        return $next($request);
	    }
	}

前置 & 后置中间件
中间件是在请求之前或者之后执行，取决于中间件的本身。例如，上面的中间件将在应用处理请求之前执行任务：所以叫前置中间件。用户的所有请求信息都在$request中包含。

如果用户输入不合法，不想要跳转到其它页面，而是返回json数据：假如中间件名为ApiAuth.php

	<?php
	
	namespace App\Http\Middleware;	
	use Closure;
	use Illuminate\Http\Request;
	
	class ApiAuth
	{
	    /**
	     * The names of the cookies that should not be encrypted.
	     *
	     * @var array
	     */
	    protected $except = [
	        //这里是此中间件不会处理的路由，如果定义到全局中间件或中间件组
	    ];
	
	    public function handle($request , Closure $next){
	        return response()->json(['error' => 'Unauthenticated.'], 401);
	    }

}
### <div id="ORM"> ORM</div> 
ORM（Object Relational mapping） 称为『对象关系映射』，是针对面向对象的编程语言（比如 PHP）和关系型数据库（比如 MySQL）的一种技术，它的目的是为了实现这两种不同类型的数据系统的数据交互。

简答点说就是：

##### 1.用一个 PHP Class 代表数据库里的一张表，这个 Class 被称为『模型』（Model）。在 Laravel 中，所有继承 Illuminate\Database\Eloquent\Model 的类就是一个模型。

	<?php

	namespace App\Models;
	
	use Illuminate\Database\Eloquent\Model;
	
	class Author extends Model
	{
	
	}

##### 2.类的一个实例表示数据库里的一条记录。

	// $author 表示数据库表 `authors` 中主键是 `1` 的那条记录
	$author = Author::find(1);

##### 3.对 Model 操作都会映射成对应的 SQL 语句，你几乎不用写原生 SQL 了

	$author->name = 'baooab'；
	$author->save();
	
	// 产生的 SQL 如下：
	update `authors` set `name` = 'baooab', `updated_at` = '2017-08-20 15:10:43' where `id` = 1;

Eloquent
Laravel 的 ORM 实现称为『Eloquent』，又称『Eloquent ORM』。Laravel 中的模型是 ORM 实现的载体，称 『Eloquent Model』，简称『Model』。
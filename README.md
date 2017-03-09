# 微博项目 —— 一个基于Angular框架实现前端MVC的单页面应用

# 技术栈

* 由于借助BootStrap可以轻松实现页面的快速搭建及响应式，所以，本项目采用bootstrap搭建页面，将主要精力放在该项目的前后端逻辑上；
* 前端MVC借助Angular + Require.js + AngularUIRouter实现，后端MVC借助Node.js + ExpressJS实现。数据库采用MongoDB + Mongoose的配合实现数据库的增删改查工作；
* 最后，借助socketio来开发websocket,来实现微博更新后的实时提醒。

# 项目文件夹：
该项目的文件夹结构如下所示：<br>
    ```javascript
		┠ app.js	 nodejs的运行文件
		┠ models	nodejs模型，里面放Mongoose数据库模型
		┠ ngApp	Angular的前端脚本，这个文件夹需要Express提供静态化。文件夹里面有Angular的四大金刚和index.html页面
			┃ ┠ ngControllers  Angular控制器
		┃ ┠ ngDirectives   Angular指令
		┃ ┠ ngServices     Angular服务
		┃ ┠ views         Angular模板
		┃ ┠ routes.js       Angular路由清单
		┃ ┠ main.js	 reqiurejs的主入口，这个文件负责引入其他文件
		┃ ┠ myapp.js	 Angular的Module
		┃ ┠ index.html	 唯一的单页面，有的公司喜欢把index.html放到最外层文件夹中
		┠   routes	nodejs路由，控制器
		┠   static	所有不需要跨域就能请求的静态资源比如images、css、js库等等。
	```
# 项目特点
## 功能
		* 注册会员
		* 验证登录
		* 发表微博
		* 桌面提醒
		* 微博评论
		* 微博评论点评、点赞
		* 删除别人的评论
		* 删除自己的微博
		* 个人主页能看见时间线
		* 个人一句话签名、站内信、头像系统(支持头像剪裁,借助graphicsmagick)

## 此项目使用的是SPA;
## 路由是RESTful风格。
	所谓的RESTful路由风格,简单老说就是利用HTTP的四种请求：GET（获取资源）、POST（新建资源，也可以用于更新资源）、PUT（更新资源）、DELETE（删除资源），来实现资源表现层的状态转换（Representational State Transfer）
。<br>
以下是改项目的路由：
	```javascript
	app.get("/" , router.showIndex);<br>
	app.use("/static",express.static("static"));<br>
	app.use("/app",express.static("ngApp"));
	app.get("/checkuser",router.checkuser);         //判断用户是否存在<br>
	app.post("/user",router.addUser);               //添加用户<br>
	app.post("/login",router.login)                 //登录<br>
	app.get("/checklogin",router.checklogin);       //检查是否已经登录<br>
	app.get("/logout",router.logout);               //退出登录<br>
	app.get("/user/:email",router.getuser);         //查询某个用户的资料<br>
	app.post("/user/:email",router.updateuser);     //查询某个用户的资料<br>
	app.post("/up",router.up);                      //上传头像<br>
	app.post("/cut",router.cut);                    //裁切头像<br>
	app.post("/posts/",function(req,res){<br>
		router.fabiao(req,res,io);<br>
	});                                             //发表说说<br>
	app.get("/posts/",router.getAllPost);           //得到所有说说<br>
	app.get("/posts/:id",router.getthepost);         //得到某一个说说<br>
	app.post("/comment/:id",router.postcomment);     //评论某一个帖子<br>
	```
# 项目起步
### 安装node & npm

[https://nodejs.org/](https://nodejs.org/)

### 安装 `cnpm`

```shell
npm --registry=http://registry.npm.taobao.org i -g cnpm
```
### 安装 `graohicsMagicK`

[http://www.graphicsmagick.org/](http://www.graphicsmagick.org/)

### 安装依赖

```shell
cnpm install
```

### 开数据库

```shell
mongod --dbpath c:/database
```

### 开启项目

```shell
node app
```
# 项目主要概述
## 基于Angular + Require.js + AngularUIRouter的项目起步：
### index.html
```javascript
<script src="/static/js/lib/require.min.js" data-main="/app/main.js"></script>
```
### main.js
```javascript
require.config({
    baseUrl: '/',
    paths: {
        'angular': '/static/js/lib/angular.min',
        'angular-ui-router': '/static/js/lib/angular-ui-router.min',
        'angular-async-loader': '/static/js/lib/angular-async-loader.min',
        'routes' : "/app/routes",
        "myapp" : "/app/myapp"
    },
    shim: {
        'angular': {exports: 'angular'},
        'angular-ui-router': {deps: ['angular']}
    }
});
 
require(['angular',"routes"], function (angular) {
    angular.element(document).ready(function () {
        angular.bootstrap(document, ['myapp']);
        angular.element(document).find('html').addClass('ng-app');
    });
});
```
### myapp.js
```javascript
define(function (require, exports, module) {
    var angular = require('angular');
    var asyncLoader = require('angular-async-loader');
 
    require('angular-ui-router');
 
    var myapp = angular.module('myapp', ['ui.router']);
 
    // initialze app module for angular-async-loader
    asyncLoader.configure(myapp);
 
    module.exports = myapp;
});
```
### routes.js
```javascript
define(["myapp"] , function (app) {
    app.config(['$stateProvider', '$urlRouterProvider', function ($stateProvider, $urlRouterProvider) {
        $urlRouterProvider.otherwise('/home');
 
        $stateProvider
            .state('home', {
                url: '/home',
                template: '测试'

            });
    }]);
});
```
## websocket 和 socketio
欢迎[关注个人官网](http://www.josietan.top)


# ΢����Ŀ ���� һ������Angular���ʵ��ǰ��MVC�ĵ�ҳ��Ӧ��

# ����ջ

* ���ڽ���BootStrap��������ʵ��ҳ��Ŀ��ٴ����Ӧʽ�����ԣ�����Ŀ����bootstrap�ҳ�棬����Ҫ�������ڸ���Ŀ��ǰ����߼��ϣ�
* ǰ��MVC����Angular + Require.js + AngularUIRouterʵ�֣����MVC����Node.js + ExpressJSʵ�֡����ݿ����MongoDB + Mongoose�����ʵ�����ݿ����ɾ�Ĳ鹤����
* ��󣬽���socketio������websocket,��ʵ��΢�����º��ʵʱ���ѡ�

# ��Ŀ�ļ��У�
����Ŀ���ļ��нṹ������ʾ��<br>
```javascript
		�� app.js	 nodejs�������ļ�
		�� models	nodejsģ�ͣ������Mongoose���ݿ�ģ��
		�� ngApp	Angular��ǰ�˽ű�������ļ�����ҪExpress�ṩ��̬�����ļ���������Angular���Ĵ��պ�index.htmlҳ��
			�� �� ngControllers  Angular������
		�� �� ngDirectives   Angularָ��
		�� �� ngServices     Angular����
		�� �� views         Angularģ��
		�� �� routes.js       Angular·���嵥
		�� �� main.js	 reqiurejs������ڣ�����ļ��������������ļ�
		�� �� myapp.js	 Angular��Module
		�� �� index.html	 Ψһ�ĵ�ҳ�棬�еĹ�˾ϲ����index.html�ŵ�������ļ�����
		��   routes	nodejs·�ɣ�������
		��   static	���в���Ҫ�����������ľ�̬��Դ����images��css��js��ȵȡ�
```
# ��Ŀ�ص�
## ����
		* ע���Ա
		* ��֤��¼
		* ����΢��
		* ��������
		* ΢������
		* ΢�����۵���������
		* ɾ�����˵�����
		* ɾ���Լ���΢��
		* ������ҳ�ܿ���ʱ����
		* ����һ�仰ǩ����վ���š�ͷ��ϵͳ(֧��ͷ�����,����graphicsmagick)

## ����Ŀʹ�õ���SPA;
## ·����RESTful���
	��ν��RESTful·�ɷ��,����˵��������HTTP����������GET����ȡ��Դ����POST���½���Դ��Ҳ�������ڸ�����Դ����PUT��������Դ����<br>
	DELETE��ɾ����Դ������ʵ����Դ���ֲ��״̬ת����Representational State Transfer��
��<br>
�����Ǹ���Ŀ��·�ɣ�
```javascript
	app.get("/" , router.showIndex);
	app.use("/static",express.static("static"));
	app.use("/app",express.static("ngApp"));
	app.get("/checkuser",router.checkuser);         //�ж��û��Ƿ����
	app.post("/user",router.addUser);               //����û�
	app.post("/login",router.login)                 //��¼
	app.get("/checklogin",router.checklogin);       //����Ƿ��Ѿ���¼
	app.get("/logout",router.logout);               //�˳���¼<br>
	app.get("/user/:email",router.getuser);         //��ѯĳ���û�������
	app.post("/user/:email",router.updateuser);     //��ѯĳ���û�������
	app.post("/up",router.up);                      //�ϴ�ͷ��
	app.post("/cut",router.cut);                    //����ͷ��
	app.post("/posts/",function(req,res){<br>
		router.fabiao(req,res,io);<br>
	});                                             //����˵˵
	app.get("/posts/",router.getAllPost);           //�õ�����˵˵
	app.get("/posts/:id",router.getthepost);         //�õ�ĳһ��˵˵
	app.post("/comment/:id",router.postcomment);     //����ĳһ������
```
# ��Ŀ��
### ��װnode & npm

[https://nodejs.org/](https://nodejs.org/)

### ��װ `cnpm`

```shell
npm --registry=http://registry.npm.taobao.org i -g cnpm
```
### ��װ `graohicsMagicK`

[http://www.graphicsmagick.org/](http://www.graphicsmagick.org/)

### ��װ����

```shell
cnpm install
```

### �����ݿ�

```shell
mongod --dbpath c:/database
```

### ������Ŀ

```shell
node app
```
# ��Ŀ��Ҫ����
## ����Angular + Require.js + AngularUIRouter����Ŀ�𲽣�
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
                template: '����'

            });
    }]);
});
```
## websocket �� socketio
		WebSocket protocol ��HTML5һ���µ�Э�顣��ʵ����������������ȫ˫��ͨ��(full-duple)��һ��ʼ��������Ҫ����HTTP������ɡ�
		HTTP��Ƴ�ʼ��ֻ�������ֱ�ӷ������󣬷�����������Ӧ����������������request���󣬷������ǲ��������ҵ������������һЩ���ݵġ�Ҳ����˵������������������󣬷������Żᷢ����Ӧ��
		���ڵ������������ǳ���ѯ����ͨ���׶��Ļ���˵�����ǿͻ��˲�ͣ�ģ�setInterval������������������Ի�ȡ���µ�������Ϣ������ġ���ͣ����ʵ����ֹͣ�ģ�ֻ�����������޷��ֱ��Ƿ�ֹͣ����ֻ��һ�ֿ��ٵ�ͣ��Ȼ����������ʼ���Ӷ��ѡ������ÿ������20�붼��һ�·���������û���˸��ҷ�վ���ţ�Ҧ����û��Ͷ���������������ǲ���������Ϣ������֪ͨ������ġ�
 
		HTML5����һ������websocket��Э�飬�����������������֪ͨ��
		�� WebSocket API��������ͷ�����ֻ��Ҫ��һ�����ֵĶ�����Ȼ��������ͷ�����֮����γ���һ������ͨ��������֮���ֱ�ӿ������ݻ��ഫ�͡��ڴ�WebSocket Э���У�Ϊ����ʵ�ּ�ʱ�������������ô���
		1. Header
		���๵ͨ��Header�Ǻ�С��-���ֻ�� 2 Bytes
		2. Server Push
		�����������ͣ����������ٱ����Ľ��յ��������request֮��ŷ������ݣ���������������ʱ���������͸��������
		socketio��һ��NodeJS�õ�npm��������websocket�ĳ��򿪷�����socketioʵ��websocket�ر��:<br>
### �������ˣ�
```javascript
var io = require("socket.io")(http);
 
app.get('/', function(req, res){
  res.sendFile(__dirname + "/index.html");
});
//������һ���м����
io.on("connect",function(socket){
//�������˳�����һ��socket����
console.log("����connect�ˣ�~~");
});
http.listen(3000, function(){   //���ﻻ����http����Ϊ��������һ��/socket.io/socket.io.js
  console.log('����3000�˿ڣ������ɣ���');
});
```
### ǰ�˽���
```javascript
<script>
  	 var socket = io();
</script>
��ʱ���ǵ�node.js����app.js�ļ�����ǰ��ҳ��index.html������һ��socket����������socket�Ѿ��׽����
	
��ӭ[��ע���˹���](http://www.josietan.top)


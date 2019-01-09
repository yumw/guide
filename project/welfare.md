## 人力资源管理平台轻应用

#### 技术栈
	
		vue+vue-router+axios+vux

### 福利平台

#### 一、如何启动项目
* 1、安装svn

* 2、注册svn账号，找项目管理员开通项目权限

* 3、新建项目文件夹，右键 => SVN checkout，输入svn下载地址，点OK下载项目文件，svn地址如下：
		
		http://192.168.2.14:8081/svn/www/welfare-vue

* 4、进入文件夹目录 /welfare-vue ，shift+鼠标右键打开powershell窗口

* 5、输入命令安装npm

		npm install -g  //全局安装
		npm install     //本地安装

* 6、运行命令启动项目

		npm run dev
	

#### 二、模块功能	
* 子模块：

	* 生日福利

	* 节日福利

	* 福利品

	* 内购商城

	* 体检福利

	* 福利活动

	* 合作商家

	* 保险福利

* 首页`菜单`配置

	* 1、登录人力资源管理后台，系统管理 => 轻应用 进行配置

	* 2、网址：

			http://192.168.127.21:8080/login		//测试环境
			http://hrprd.liby.com.cn/login  		//生产环境

	* 3、配置规则

			1、菜单分级：点击添加按钮 => 选择父类轻应用时，不选择则为一级轻应用，福利平台、共享服务等属于一级
			2、是否外链：配置二级菜单时此项为必选，决定移动端点击菜单时是跳路由还是外链
			3、可见范围：按需配置
			4、封面图片：此项必传
			5、移动端地址：外链填完整url，非外链填路由url，比如/home

	* 4、移动端配置福利平台入口时加上 ?lappType=35，接口参数lappType与 `轻应用编号` 相同；本项目可以动态获取url上的lappType参数，请求接口，获取配置的菜单列表；

			注意：
			1、此点说的福利平台入口为 `嘟嘟轻应用入口`，在另一个地方配置
			2、测试环境的 lappType 值与正式环境的值可能不同


	

#### 三、测试环境与生产环境url配置

* 1、字段
		LOCAL  本地
		BETA   测试
		UAT    测试
		PRD    生产

* 2、basicURL
	
	* 1、接口

			http://hruat.liby.com.cn/api			//测试
			http://hrapiprd.liby.com.cn:866/api		//生产

		内购商城（智选）接口域名

			http://hruat.liby.com.cn  				//测试
			http://hrapiprd.liby.com.cn:866 		//生产

	* 2、图片url

			http://192.168.127.21:8081/images/		//测试
			http://hrprd.liby.com.cn/filepath/ 		//生产
	
	* 3、文件url（保险福利）

			http://192.168.127.21:8081/files/				//测试
			http://hrprd.liby.com.cn/filepath/insurance/	//生产

	* 4、后台上传的文件的url

			http://192.168.127.21:8081/files/		//测试
			http://hrprd.liby.com.cn/filepath/		//生产

* 3、完整代码
	
welfare-vue/util/config.js

``` javascript

	const ENV = (function() {
		if (location.href.indexOf('http://localhost')==0 
		|| location.href.indexOf('http://127.0.0.1')==0 
		|| location.href.indexOf('http://172.15.11.219')==0){
			return "LOCAL";
		}else if (location.href.indexOf('http://hruat.liby.com.cn')==0){
			return "BETA";
		}else if (location.href.indexOf('http://hruat.liby.com.cn')==0){
			return "UAT";
		}else {
			return "PRD";
		}
	})();

	const Config={
		LOCAL:{
			host: "http://192.168.195.73:8080",
			mallHost: "https://mmalluat.liby.com.cn",
			imgUrl:"http://192.168.127.21:8081/images/",
			fileUrl:"http://192.168.127.21:8081/files/",
			activeFileUrl:"http://192.168.127.21:8081/files/"
		},
		BETA:{
			host: "http://hruat.liby.com.cn/api",
			mallHost: "http://hruat.liby.com.cn",
			imgUrl:"http://192.168.127.21:8081/images/",
			fileUrl:"http://192.168.127.21:8081/files/",
			activeFileUrl:"http://192.168.127.21:8081/files/"
		},
		UAT:{
			host: "http://hruat.liby.com.cn/api",
			imgUrl:"http://192.168.127.21:8081/images/",
			fileUrl:"http://192.168.127.21:8081/files/",
			activeFileUrl:"http://192.168.127.21:8081/files/"
		},
		PRD:{
			host: "http://hrapiprd.liby.com.cn:866/api",
			mallHost: "http://hrapiprd.liby.com.cn:866",
			imgUrl:"http://hrprd.liby.com.cn/filepath/",
			fileUrl:"http://hrprd.liby.com.cn/filepath/insurance/",
			activeFileUrl:"http://hrprd.liby.com.cn/filepath/"
		}
	};

	export default {
		host: Config[ENV].host,
		mallHost: Config[ENV].mallHost,
		imgUrl: Config[ENV].imgUrl,
		fileUrl: Config[ENV].fileUrl,
		activeFileUrl: Config[ENV].activeFileUrl,
	}
```

main.js
```javascript

	/* 引入 */
	import global_ from './util/config'
	/* 全局变量 */
	Vue.prototype.GLOBAL = global_
	Vue.prototype.imgURL = global_.imgUrl

```


使用

``` javascript

	let params = {
	    type : "welfare",
	  	openId: self.GLOBAL.userinfo.openId
	}
	self.$http.post(self.GLOBAL.host+'/auth',params)
	.then(function (response) {
	         
	    })
	    .catch(function (error) {
	       
	    });
	}

```


#### 四、前端发版

* 1、下载ftp，安装

* 2、打开ftp，站点管理器 => 新站点
	
	* 协议：FSTP

	* 测试站点：192.168.120.21  端口：8080

	* 生产站点：192.168.120.250  端口：8080

	* 登录类型：正常

* 3、输入账号密码，连接

* 4、进入项目文件目录

		/usr/share/nginx/html/lapp/welfare

* 5、powershell窗口运行命令打包
		
		npm run build

* 6、打包完成后用本地项目 `welfare-vue/dist` 文件夹全部文件替换 `ftp项目文件目录` 内容，传输成功，发版完毕

#### 五、嘟嘟入口配置

* 1、正式环境，登录嘟嘟轻应用配置，配置入口url

		http://hrapiprd.liby.com.cn:866/lapp/welfare/index.html?lappType=37   //正式

* 2、测试环境，登录ftp，将/user/share/nginx/html/lapp/main.js下载下来，修改对应的入口a标签

		http://hruat.liby.com.cn/lapp/welfare/index.html#/home?lappType=35    //测试



### 共享服务

* 1、svn

		http://192.168.2.14:8081/svn/www/shared-services

* 2、ftp目录

		/usr/share/nginx/html/lapp/shared-services

* 3、basicURL
	
	* 1、接口

			http://hruat.liby.com.cn/api			//测试
			http://hrapiprd.liby.com.cn:866/api		//生产

	* 2、图片url

			http://192.168.127.21:8081/images/		//测试
			http://hrprd.liby.com.cn/filepath/ 		//生产

* 4、配置入口url

		//正式
		http://hrapiprd.liby.com.cn:866/lapp/shared-services/index.html?lappType=38
		//测试       
		http://hruat.liby.com.cn/lapp/shared-services/index.html#/home?lappType=62 	      

		lappType的值可能会改变，参考福利平台 * 首页菜单配置 *


### 考勤打卡

* 1、svn

		http://192.168.2.14:8081/svn/www/attendance

* 2、ftp目录

		/usr/share/nginx/html/lapp/attendance

* 3、basicURL
	
	* 1、接口

			http://hruat.liby.com.cn/api			//测试
			http://hrapiprd.liby.com.cn:866/api		//生产
			http://192.168.129.230:8080/api         //外包人员打卡

	* 2、图片url

			http://192.168.127.21:8081/images/		//测试
			http://hrprd.liby.com.cn/filepath/ 		//生产

* 4、配置入口url

		//正式
		http://hrapiprd.liby.com.cn:866/lapp/attendance/index.html
		//测试       
		http://hruat.liby.com.cn/lapp/attendance/index.html 			



### 微学通积分

* 1、svn

		http://192.168.2.14:8081/svn/www/train-integral


* 2、ftp目录

		/usr/share/nginx/html/lapp/train-integral

* 3、basicURL
	
	* 1、接口

			http://hruat.liby.com.cn/api			//测试
			http://hrapiprd.liby.com.cn:866/api		//生产

	* 2、图片url
	
			http://192.168.127.21:8081/images/		//测试
			http://hrprd.liby.com.cn/filepath/ 		//生产

* 4、配置入口url

		//正式
		http://hrapiprd.liby.com.cn:866/lapp/train-integral/index.html
		//测试       
		http://hruat.liby.com.cn/lapp/train-integral/index.html 			 




PhoneGap Install & Plugin
=========================
[![npm version](http://img.shields.io/badge/npm%20package-1.0.1-brightgreen.svg)](http://shields.io/)

PhoneGap的iOS插件，Mark下以免日后需要的时候忘记了。    
  
About PhoneGap
==============
PhoneGap，这玩意就是原来是某公司的，然后被Adobe收了，然后被捐给Apache基金会开源了。个人理解大概就是个跨平台的中间件，可以使用HTML、JS、CSS代码编写Web App并通过它调用一些特有平台上的特性。

PhoneGap Install
================
安装前，确保您已经安装了 NodeJS，然后打开命令行运行以下命令:  
`$ sudo npm install -g cordova`

##PhoneGap Creat
$ cd到项目文件夹，执行一下命令。第一个参数是项目的文件夹名，第二个参数是项目的id，第三个参数是项目工程名。  
`$ cordova create hello com.example.hello HelloWorld`  

$ cd进刚刚新建的hello目录下，添加平台支持  
`$ cordova platform add ios`

##build & Run
`$ cordova build`  
**注意，这里貌似是一个坑。在iOS的平台代码中编写的HTML、JS、CSS代码，如果不是用命令行build，直接在Xcode里跑的话，没有把代码合并到主项目代码并加到生成的包里面。至少我这边没有！**

PhoneGap Plugin
===============
###首先在config.xml配置文件中注册插件  

`<feature name="Echo">`  
`<param name="ios-package" value="Echo" />`  
`</feature>`

###网页 JS 代码调用插件

`function phoneGapDemoFunc(param1, param2) {  `  
`var parameter = [param1, param2];// 调用插件"Echo"中的"echo"方法，参数以数组形式传入  `  
`Cordova.exec(function(result){}, function(error){}, "Echo", "echo", parameter);  `  
`}`

###iOS 插件代码
/********* Echo.h Cordova Plugin Header *******/

`#import <Cordova/CDV.h>`

`@interface Echo : CDVPlugin`

`- (void)echo:(CDVInvokedUrlCommand *)command;`

`@end`

/********* Echo.m Cordova Plugin Implementation *******/

`#import "Echo.h"`

`#import <Cordova/CDV.h>`

`@implementation Echo`

    - (void)echo:(CDVInvokedUrlCommand *)command { 
  
      CDVPluginResult * pluginResult = nil; 
      NSString * echo = [command.arguments objectAtIndex:0];

	    if (echo != nil && [echo length] > 0) {
	    pluginResult = [CDVPluginResult resultWithStatus:CDVCommandStatus_OK messageAsString:echo];
	    } else {
  		pluginResult = [CDVPluginResult resultWithStatus:CDVCommandStatus_ERROR];
	    }
	    [self.commandDelegate sendPluginResult:pluginResult callbackId:command.callbackId];
    }
    
    @end
    
  本地代码执行成功或失败通过通过回调函数可以回调JS代码执行Web内容刷新等。

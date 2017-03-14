
# 项目流程

[TOC]

### 大致流程
* git建项
* 项目根目录建立README.md或者git wiki内,内容为项目基本的说明、构建、打包、部署方法
* 文档内基础必须内容：
	* 项目download下来后，如何安装初始化（初始化过程中人为干涉部分也需要详细说明)
	* 如何启动调试
	* 如何打包
	* 部署命令
		* 本地传入svn如（可以在项目根目录下创建svn.sh，别人只需要执行就能上传svn）：
		```
			svn delete svn://192.168.1.40/MSI-FE/trunk/wxmxdzc/ -m "delete auto sh"
			svn mkdir svn://192.168.1.40/MSI-FE/trunk/wxmxdzc/ -m "create auto sh"
			svn import ./dist/ svn://192.168.1.40/MSI-FE/trunk/wxmxdzc// -m "upload auto sh"
		```
		* 服务器上同步svn命令如：
		```
		svn export svn://svn.nonobank.com/MSI-FE/trunk/wxmxdzc /usr/share/nginx/www/trunk/wxmxdzc --force  
		```
	* 部署后的完整访问方式：
		* 访问地址：`https://m.nonobank.com/wxmxdzc/index.html`
		* 参数列表的说明如
		
		参数名|是否可选|说明
		---|---|---
		sessionId|必须|用户的sessionId|
		ver|可选|版本号|
		mock|可选|当需要启动本地mock接口数据时填写1|

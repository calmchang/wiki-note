[TOC]

## 工作流程及规范指导
* 创建工程名：myProject
* 创建git源代码工程:git/myProject
* 创建git对外的文档工程:git/com-doc/myProject
* 开发
* 测试
* 上线
* 迭代
	* 创建git分支 feature/分支名
	* 开发
	* merge master into 分支
	* 测试
	* 上线
	* 创建master分支到backup进行备份
	* merge 分支 into master
	* 删除分支
* 紧急bug
	* 创建git分支 hotfix/分支名
	* 开发
	* merge master into 分支
	* 测试
	* 上线
	* 创建master分支到backup进行备份
	* merge 分支 into master
	* 删除分支



## 源代码工程规范
* 工程目录下包含readme.md，主要内容（保证他人能按照文档顺利运行）：
	* 怎么构建启动工程？如：
		```bash
			需要:node v4.2.2
			nvm use v4.2.2 
			sudo npm install
			调试:sudo gulp dev
		```

	* 工程怎么打包部署？如：
		```bash
			打包: sudo sh package.sh
			发布到svn: sh svn.sh
			服务器发布：svn export svn://192.168.1.40/MSI-FE/trunk/bld/app/dejversion160 /usr/share/nginx/www/trunk/bld/app/dejversion160 --force
		```
	* 重要的注意事项

* 架构文档readme文件夹下
	* 整体架构图
	* 各个小系统模块的逻辑流程图、架构图

## 对外文档工程规范
* 主要内容
	* 外部对接相关的信息如：jsbridge、url参数列表、页面url地址
	* 业务逻辑流程图

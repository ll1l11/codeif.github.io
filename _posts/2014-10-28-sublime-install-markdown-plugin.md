---
layout: post
title: sublime下安装markdown的插件
---

使用sublime编写预览markdown文件, 以下操作在sublime2有效, sublime3没有验证, mac中将ctrl换为command就可以了

### 语法高亮

默认的配色方案可能不高亮, 换为  Color Scheme -> Sunburse

### 在浏览器中预览

安装 MarkdownBuild 插件


1. 安装package control组件

	* 按Ctrl+`调出console
	* 粘贴以下代码到底部命令行并回车
	
		import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) 	else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
	
	* 重启 sublime
	* package settings中看到package control这一项，则安装成功.


2. ctrl + shif + p 调出命令面板

	* 输入install, 选择install package
	* 输入markdown, 选择MarkdownBuild插件安装


装完 MarkdownBuild 插件后, 按CTRL+b就可以在默认浏览器中预览md结尾的文件了
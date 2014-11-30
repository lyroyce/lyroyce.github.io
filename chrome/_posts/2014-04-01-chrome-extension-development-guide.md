---
layout: post
title: 简明Chrome Extension开发教程
---

基础
------
一个Chrome Extension是一个包含HTML、JavaScript、CSS的特殊压缩包，后缀名为crx。它其实也是网页文件组成，可以使用浏览器提供给网页的所有API，来扩展Chrome浏览器的功能。

文件结构
------
一个典型的扩展的文件结构如下。不难看出，这和制作一个网页或者网站差不多。

	my-extension
	|- js
	|   |- bg.js
	|   |- content.js
	|- css
	|   |- style.css
	|- img
	|   |- logo.png
	|- manifest.json

描述文件
-----
一个扩展必须在根目录包含一个名为`manifest.json`的描述文件。它包含了很多对扩展的定义和配置。

	{
		"name": "My Extension",
		"version": "2.1",
		"description": "An image viewer.",
		"permissions": ["tabs", "http://*/*", "https://*/*"],
		"background": {
			"scripts": ["js/bg.js"]
		},
		"browser_action": {
			"default_icon": "img/logo.png"
		},
		"content_scripts": [
	        {
	            "matches" : [ "http://*/*","https://*/*"],
	            "run_at" : "document_end",
	            "js": ["js/content.js"],
	            "css": ["css/style.css"]
	        }
	    ]
	}

内容脚本和后台脚本
------
Chrome扩展中可以运行两种脚本，内容脚本和后台脚本。内容脚本的生命周期和网页一致，而后台脚本的生命周期则和浏览器一致。也就是说，内容脚本会在每一个tab都分别加载，而后台脚本则只会在后台运行一份（独立于tab）。

内容脚本适用于完成和网页打交道的任务，例如用来找出网页中的图片。`manifest.json`中的下面这段定义就指定了一个内容脚本。它匹配所有http或https开头的网页，在网页加载完成之后运行`js/content.js`。
	
	"content_scripts": [
        {
            "matches" : [ "http://*/*","https://*/*"],
            "run_at" : "document_end",
            "js": ["js/content.js"]
        }
    ]

而后台脚本则用来完成一些和网页无关的或者公共的任务。类似地，下面这段定义则指定一个后台脚本`js/bg.js`。

	"background": {
		"scripts": ["js/bg.js"]
	}

交互和API
------
既然内容脚本和后台脚本的上下文不一样，它们之间的交互就不能直接进行函数调用了，而是需要使用Chrome API了。例如，我们可以在内容脚本中用`chrome.extension.sendRequest`API来向后台脚本发送一条消息：

	// js/content.js
	chrome.extension.sendRequest('Hello', function(response){
			console.log(response);
		});

相应地，我们也需要在后台脚本中来接受和处理消息，这里我们使用了回调：

	// js/bg.js
	chrome.extension.onRequest.addListener(
		function(request, sender, callback){
			callback(request + ' World');
		});

扩展开关
------
一个典型的扩展会在Chrome右上角扩展栏添加相应图标按钮，来作为扩展开关。当然你也可以添加右键菜单来触发扩展，或者不需要开关而在背后运行扩展。

`manifest.json`中的下面这段定义指定了一个扩展栏图标。
	
	"browser_action": {
		"default_icon": "img/logo.png"
	}

由于扩展按钮不属于任何tab，所以它的相应代码需要放在后台脚本中。下面这段代码响应了该按钮的点击事件，并向内容脚本发送了一条消息：

	// js/bg.js
	chrome.browserAction.onClicked.addListener(
	    function(tab) {
	    	chrome.tabs.sendRequest(tab.id, 'Great Job');
	    });

注意，这里使用的`chrome.tabs.sendRequest`和之前的`chrome.extension.sendRequest`有所不同。它是由后台脚本向内容脚本发送，第一个参数指定向具体哪个tab的内容脚本发送。由于内容脚本这次是被动接收，所以还需要注册一个接收函数。

	// js/content.js
	chrome.extension.onRequest.addListener(function(response){
			console.log(response);
		});

结束语
----
好了，到这里，一个简单Chrome扩展的雏形就完成了。它会在任意网页加载完成后，在控制台打印出`Hello World`。它也会在扩展按钮被点击时，在控制台打印出`Great Job`。

接下来，是时候发挥你的想象力，改造内容脚本，创造一个属于你的独特的扩展吧。
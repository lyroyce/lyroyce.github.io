---
layout: post
title: 简明Chrome Extension开发教程
---

基础
------
一个Chrome Extension是一个包含HTML、JavaScript、CSS的特殊压缩包，后缀名为crx。它其实也是网页，可以使用浏览器提供给网页的所有API，来扩展Chrome浏览器的功能。

描述文件
-----
一个扩展必须包含一个名为`manifest.json`的描述文件。

	{
		"name": "My Extension",
		"version": "2.1",
		"description": "Gets information from Google.",
		"background": {
			"scripts": ["js/bg.js"]
		},
		"permissions": ["tabs", "http://*/*", "https://*/*"],
		"browser_action": {
			"default_icon": "img/logo.png"
		},
		"content_scripts": [
	        {
	            "matches" : [ "http://*/*","https://*/*"],
	            "run_at" : "document_end",
	            "js": ["js/content.js"],
	            "css": ["css/focus.css"]
	        }
	    ]
	}

扩展入口
------
一个扩展可以选择在Chrome右上角扩展栏添加一个图标按钮来进行触发。`manifest.json`中的这段定义添加了一个图标。
	
	"browser_action": {
		"default_icon": "img/logo.png"
	}

下面这段位于`js/bg.js`中的JavaScript响应了按钮的点击事件，并使用了Chrome API向当前的tab发送了一条消息：

	chrome.browserAction.onClicked.addListener(
	    function(tab) {
	    	// do something here
	        chrome.tabs.sendRequest(tab.id, 'I'm ready');
	    });

内容脚本和后台脚本
------
`manifest.json`中的`content_scripts`定义了每一个tab都需要单独加载的脚本，而`background`则定义了可以在后台运行的脚本（独立于tab）。

上述的扩展按钮不属于任何tab，所以它的相应代码需要放在后台脚本中。

交互和API
------
如果扩展按钮的点击需要和页面相关，比如触发在页面中的视图变化，那么就涉及到和内容脚本的交互了。这时，我们就需要用到`chrome.tabs.sendRequest`这个API来向内容脚本发送消息。相应地，我们也需要在内容脚本中来接受和处理消息：

	chrome.extension.onRequest.addListener(
		function(message){
			console.log(message);
		});
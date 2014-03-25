---
layout: post
title: 用Composer发布PHP开源项目
---

开源
-------
首先当然是开源你的项目，比如在GitHub上。你可能需要考虑选择一种开源协议，例如BSD，MIT，GPL等，参见[开源协议浅析]({% post_url 2014-01-12-open-source-license %})。

让Composer认识你的项目
-------
在项目根目录创建composer.json文件，下面是一个典型的例子：

	{
	    "name": "monolog/monolog",
	    "description": "Logging for PHP 5.3",
	    "keywords": ["log","logging"],
	    "homepage": "http://github.com/Seldaek/monolog",
	    "license": "MIT",
	    "authors": [
	        {
	            "name": "Jordi Boggiano",
	            "email": "j.boggiano@seld.be",
	            "homepage": "http://seld.be",
	        }
	    ],
	    "require": {
	        "php": ">=5.3.0"
	    }
	}

发布项目
-------
Composer默认只有一个repository，就是[packagist.org](http://packagist.org)。你只需要有一个packagist的账号，填写你的项目的URL，然后提交即可。

自动更新
-------
packagist默认一天进行一次更新。如果你想要在每次push之后自动更新到Composer，可以在GitHub项目设置中添加一个packagist的webhook。

版本管理
-------
项目的版本默认是`dev-master`，它指向master branch。为了更好的进行版本控制，有两种方式可以指定可比较的新版本：

- 给dev-master取别名

		"extra": {
	        "branch-alias": {
	            "dev-master": "1.0-dev"
	        }
	    }

- 在版本控制系统（如GitHub）中创建Tag（或Branch）
		
		$ git tag -a 1.4.0 -m 'my version 1.4'
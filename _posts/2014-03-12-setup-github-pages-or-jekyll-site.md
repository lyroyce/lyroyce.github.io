---
layout: post
title: 搭建GitHub Pages/Jekyll站点
---

准备环境
-------
- Ruby
- RubyGems
- Linux, Unix, or Mac OS X

**NOTE:** Windows不是官方支持，但如果你坚持，依然可以参阅[这篇文章](http://jekyllrb.com/docs/windows/#installation)。

建立Jekyll站点
-------

    ~ $ gem install jekyll                  # 安装Jekyll
    ~ $ jekyll new my-awesome-site          # 在当前目录创建名为my-awesome-site的新站点
    ~ $ cd my-awesome-site
    ~/my-awesome-site $ jekyll serve -w     # 在本地生成并运行站点，-w用来监控修改以便随时更新

**NOTE:** 如果你想在已有的GitHub Repository中初始化Jekyll站点，可以在第二条命令处加上`--force`来强制生成站点。

**NOTE:** 你也可以选择以一些[已有的主题](http://jekyllthemes.org/)为基础，来建立你的站点。

熟悉Jekyll目录结构
------
| FILE / DIRECTORY  |DESCRIPTION|
|-------------------|-----------|
|_config.yml|Stores configuration data. Many of these options can be specified from the command line executable but it’s easier to specify them here so you don’t have to remember them.|
|_drafts|Drafts are unpublished posts. The format of these files is without a date: title.MARKUP. Learn how to work with drafts.|
|_includes|These are the partials that can be mixed and matched by your layouts and posts to facilitate reuse. The liquid tag  {{{% include file.ext %}}} can be used to include the partial in  _includes/file.ext.|
|_layouts|These are the templates that wrap posts. Layouts are chosen on a post- by-post basis in the YAML front matter, which is described in the next section. The liquid tag  {{ content }} is used to inject content into the web page.|
|_posts|Your dynamic content, so to speak. The naming convention of these files is important, and must follow the format: YEAR-MONTH-DAY-title.MARKUP. The permalinks can be customized for each post, but the date and markup language are determined solely by the file name.|
|_data|Well-formatted site data should be placed here. The jekyll engine will autoload all yaml files (ends with .yml or .yaml) in this directory. If there's a file members.yml under the directory, then you can access contents of the file through site.data.members.|
|_site|This is where the generated site will be placed (by default) once Jekyll is done transforming it. It’s probably a good idea to add this to your .gitignore file.|
|index.html and other HTML, Markdown, Textile files|Provided that the file has a YAML Front Matter section, it will be transformed by Jekyll. The same will happen for any .html, .markdown,  .md, or .textile file in your site’s root directory or directories not listed above.|
|Other Files/Folders|Every other directory and file except for those listed above—such as css and images folders,  favicon.ico files, and so forth—will be copied verbatim to the generated site. There are plenty of sites already using Jekyll if you’re curious to see how they’re laid out.|

配置Jekyll站点
------
Jekyll站点的配置存放在`_config.yml`中，有时我们需要进行一些简单的修改。
比如，下面的配置项可以在生成站点时，排除掉`node_modules`文件夹，并包含默认隐藏的`.htaccess`文件：

    exclude: ['gulpfile.js', 'node_modules', 'package.json']
    include: ['.htaccess']

编写文章
-------
接下来就是写文章了。Jekyll站点的文章存放在`_posts`下，并遵循下面的命名规范：

    YEAR-MONTH-DAY-title.MARKUP
    例如：
    2011-12-31-new-years-eve-is-awesome.md
    2012-09-12-how-to-write-a-blog.textile

所有的文件都应以一段[YAML front-matter](http://jekyllrb.com/docs/frontmatter/)开头：

    ---
    layout: post
    title: How to Create a Jekyll Blog
    ---

文章内容支持[Markdown](http://daringfireball.net/projects/markdown/)和[Textile](http://textile.sitemonks.com/)两种语法，以Markdown为例：
    
    A First Level Header
    ====================

    A Second Level Header
    ---------------------

    Now is the time for all good men to come to
    the aid of their country. This is just a
    regular paragraph.

    ### Header 3

    > This is a blockquote.
    > 
    > This is the second paragraph in the blockquote.
    >
    > ## This is an H2 in a blockquote

List：
    
    *   A list item.

    With multiple paragraphs.

    *   Another item in the list.

Link：

    This is an [example link](http://example.com/).

Image：
    
    ![alt text](/path/to/img.jpg "Title")

部署到GitHub Pages
-----
在本地预览过你的站点后，就可以开始最后的部署了。

- 如果是创建个人站点，要在GitHub里创建名为`your-username.github.io`的Repository，然后把我们之前做好的Jekyll站点放在`master`分支上，几分钟之后你就可以在`your-username.github.io`访问到了。

- 如果是创建项目站点，要在该项目中创建`gh-pages`分支，在该分支中存放Jekyll站点，然后就可以在`your-username.github.io/projectname`来访问了。

**Note：** 有些本地文件是不需要上传的，比如`_site`目录，它会由GitHub负责生成，我们需要确保在`.gitignore`中忽略它们。

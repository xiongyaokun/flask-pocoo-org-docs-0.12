**_写在开头：使用Pycharm创建installation.md文件，Pycharm居然不能识别，好奇怪，不得不改名了!_**

![image](D:\Python\flask\jianshu\flask.png)
###Table Of Contents
[Installation](http://flask.pocoo.org/docs/0.12/installation/#)
- [(虚拟环境)virtualenv](http://flask.pocoo.org/docs/0.12/installation/#virtualenv)
- [(全系统安装)System-Wide Installation](http://flask.pocoo.org/docs/0.12/installation/#system-wide-installation)
- [试用最新版本](http://flask.pocoo.org/docs/0.12/installation/#living-on-the-edge)
- [在Windows下的pip和setuptools](http://flask.pocoo.org/docs/0.12/installation/#pip-and-setuptools-on-windows)

###版本
[开发版](http://flask.pocoo.org/docs/dev/installation/)（不稳定）
[Flask 0.12.x](http://flask.pocoo.org/docs/0.12/installation/)(稳定版)
[Flask 0.11.x](http://flask.pocoo.org/docs/0.11/installation/)
[Flask 0.10.x](http://flask.pocoo.org/docs/0.10/installation/)

###PALLETS
[The Pallets Projects are a collection of Python web development libraries.](http://www.palletsproject.com/)

###相关主题(Related Topics)
- [Tutorial](http://flask.pocoo.org/docs/0.12/tutorial/)
  - 上一篇[Toturial](http://flask.pocoo.org/docs/0.12/tutorial/)
  - 下一篇[步骤零：创建文件夹](http://flask.pocoo.org/docs/0.12/tutorial/folders/)


##Flaskr简介

这篇教程将要演示一个叫Flaskr的应用，实质上，它将于做下面的事情：
1. 可以是在配置中指定的用户登录和登出，只允许一个用户；
2. 当用户登录的时候，他可以增加新的条目，这些条目由标题和一些HTML文本组成。这些HTML
没有被过滤，因为我们相信这个用户。
3. 索引页面按照时间排序，展示了到目前为止所有的条目（最新的展示在最前面），如果用户已经登陆的话，可以
在这里添加新条目。

在这个应用中，我们会直接使用SQLite3，因为对于这个小应用来说，足够使用了。对于更大的一些应用，使用SQLAlchemy会更好，
因为它可以用更只能的方式连接数据库，允许你一次或者多次定位不同的关系型数据库。也许你会选择一个非关系型
数据库，如果你的数据适合使用的话。

这里有个最终版应用的截图：
![image](D:\Python\flask\jianshu\Tutorial\flaskr.png)

继续阅读[步骤零：创建文件夹](http://flask.pocoo.org/docs/0.12/tutorial/folders/#tutorial-folders)
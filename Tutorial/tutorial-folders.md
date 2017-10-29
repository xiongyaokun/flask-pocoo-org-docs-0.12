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
[文档预览](http://flask.pocoo.org/docs/0.12/)
- [Tutorial](http://flask.pocoo.org/docs/0.12/tutorial/)
  - 上一篇[Flaskr简介](http://flask.pocoo.org/docs/0.12/tutorial/introduction/)
  - 下一篇[步骤一：数据库方案](http://flask.pocoo.org/docs/0.12/tutorial/schema/)


##步骤零：创建文件夹

在你开始之前，你需要创建这个应用所需要的文件夹：

```
/flaskr
    /flaskr
        /static
        /templates
```
这个应用将会以Python包的形式安装和运行。这是推荐的创建和运行Flask应用的方式。在这篇教程中，
你会明白的知道怎样运行 **flaskr** 。现在你需要创建文件夹结构。在接下来的步骤中，你会创建数据库方案
和主要的模块。

作为一个快速的旁注，在**static**文件夹中的文件，可以允许用户以HTTP的方式使用。这里放置了CSS和JavaScript文件。
在**templates**文件夹中，Flask将会在其中寻找[Jinjia2](http://jinja.pocoo.org/)模板，
接下来你会看到示例。

现在你该开始下一步了：[步骤一：数据库方案](http://flask.pocoo.org/docs/0.12/tutorial/schema/#tutorial-schema)
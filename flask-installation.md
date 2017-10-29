**_写在开头：使用Pycharm创建installation.md文件，Pycharm居然不能识别，好奇怪，不得不改名了!_**

![image](D:\Python\flask\jianshu\flask.png)
### Table Of Contents
[Installation](http://flask.pocoo.org/docs/0.12/installation/#)
- [(虚拟环境)virtualenv](http://flask.pocoo.org/docs/0.12/installation/#virtualenv)
- [(全系统安装)System-Wide Installation](http://flask.pocoo.org/docs/0.12/installation/#system-wide-installation)
- [试用最新版本](http://flask.pocoo.org/docs/0.12/installation/#living-on-the-edge)
- [在Windows下的pip和setuptools](http://flask.pocoo.org/docs/0.12/installation/#pip-and-setuptools-on-windows)

### 版本
[开发版](http://flask.pocoo.org/docs/dev/installation/)（不稳定）
[Flask 0.12.x](http://flask.pocoo.org/docs/0.12/installation/)(稳定版)
[Flask 0.11.x](http://flask.pocoo.org/docs/0.11/installation/)
[Flask 0.10.x](http://flask.pocoo.org/docs/0.10/installation/)

### PALLETS
[The Pallets Projects are a collection of Python web development libraries.](http://www.palletsproject.com/)

### 相关主题(Related Topics)
[Documentation overview](http://flask.pocoo.org/docs/0.12/)
- 上一篇[Forword for Experienced Programmers](http://flask.pocoo.org/docs/0.12/advanced_foreword/)
- 下一篇[Quickstart](http://flask.pocoo.org/docs/0.12/quickstart/)

## 安装
Flask依赖于一些外部库，如[Werkzeug](http://werkzeug.pocoo.org/)和[Jinjia2](http://jinja.pocoo.org/).Werkzeug是
一个WSGI的工具包，WSGI是一套介于移动应用和许多种服务器之间标准的Python接口，用于开发和部署。Jinjia2是一套渲染模板。

所以，怎么样才能快速的在你的电脑上得到这些呢？有许多方法可以使用，但是最便捷的方法是使用virtualenv，所以，让我们首先看一下这个吧！

在开始前你需要安装Python2.6或者更新的版本，如果你使用的是Python3，请查看[Python3 Support](http://flask.pocoo.org/docs/0.12/python3/#python3-support

## Virtualenv
也许Virtualenv就是那个你会喜欢的工具，
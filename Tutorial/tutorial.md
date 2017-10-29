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
- 上一篇[Quickstart](http://flask.pocoo.org/docs/0.12/quickstart/)
- 下一篇[Introducing Flaskr](http://flask.pocoo.org/docs/0.12/tutorial/introduction/)


## 教程

你是不是想用Python和Flask开发一个一个应用啊？在这里你有机会跟着例子来学习。在这个教程中，
我们将要创建一个小型的微型博客应用。它仅仅支持一个用户，而且只能进行文本输入，没有反馈和评论功能，
但是它也包含了许多让你开始一个开发的所有功能。我们使用Flask和SQLite作为数据库(which comes out 
of the box with Python)，所以你不需要其他东西了。

如果你需要所有的源代码或者是为了比较，请查看[example source](https://github.com/pallets/flask/tree/master/examples/flaskr/)

- [介绍Flaskr](http://flask.pocoo.org/docs/0.12/tutorial/introduction/)
- [步骤零：创建文件夹](http://flask.pocoo.org/docs/0.12/tutorial/folders/)
- [步骤一：数据库模式](http://flask.pocoo.org/docs/0.12/tutorial/schema/)
- [步骤二：应用程序创建](http://flask.pocoo.org/docs/0.12/tutorial/setup/)
- [步骤三：以包的形式安装flaskr](http://flask.pocoo.org/docs/0.12/tutorial/packaging/)
- [步骤四：数据库连接](http://flask.pocoo.org/docs/0.12/tutorial/dbcon/)
- [步骤五：创建数据库](http://flask.pocoo.org/docs/0.12/tutorial/dbinit/)
- [步骤六：视图函数](http://flask.pocoo.org/docs/0.12/tutorial/views/)
  - [显示条目](http://flask.pocoo.org/docs/0.12/tutorial/views/#show-entries)
  - [增加新条目](http://flask.pocoo.org/docs/0.12/tutorial/views/#add-new-entry)
  - [登陆和登出](http://flask.pocoo.org/docs/0.12/tutorial/views/#login-and-logout)
- [步骤七：模板](http://flask.pocoo.org/docs/0.12/tutorial/templates/)
  - [layout.html](http://flask.pocoo.org/docs/0.12/tutorial/templates/#layout-html)
  - [show_entries.html](http://flask.pocoo.org/docs/0.12/tutorial/templates/#show-entries-html)
  - [login.html](http://flask.pocoo.org/docs/0.12/tutorial/templates/#login-html)
- [步骤八：添加样式](http://flask.pocoo.org/docs/0.12/tutorial/css/)
- [彩蛋：应用测试](http://flask.pocoo.org/docs/0.12/tutorial/testing/)
  - [向flaskr添加测试](http://flask.pocoo.org/docs/0.12/tutorial/testing/#adding-tests-to-flaskr)
  - [运行测试](http://flask.pocoo.org/docs/0.12/tutorial/testing/#running-the-tests)
  - [测试+设置工具](http://flask.pocoo.org/docs/0.12/tutorial/testing/#testing-setuptools)
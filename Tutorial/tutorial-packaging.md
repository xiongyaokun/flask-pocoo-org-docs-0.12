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
  - 上一篇[步骤二：初始化应用](http://flask.pocoo.org/docs/0.12/tutorial/setup/)
  - 下一篇[步骤四：数据库连接flaskr](http://flask.pocoo.org/docs/0.12/tutorial/dbcon/)
  

##以包的形式安装flaskr
Flask现在提供了对Click的内置支持。[Click](http://click.pocoo.org/)给Flask提供了加强的可扩展的命令行功能。
在这篇教程的后部分，你将会看到怎么扩展Flask的命令行界面。

执行Flask应用程序的一个有用的方法是，根据[Python Packaging Guide](https://packaging.python.org/)
来安装你的应用。目前来说，需要在根目录下创建两个新文件：**setup.py**和**MANIFEST**.
同时，你也需要在**flaskr/flaskr**目录下添加一个**__init__.py**文件，使flaskr变成一个包。
更改之后，你的代码结构是这样的：
```python
/flaskr
    /flaskr
        __init__.py
        /static
        /templates
        flaskr.py
        schema.sql
    setup.py
    MANIFEST.in
```
**flaskr**下的**setup.py**文件的内容如下：
```python
from setuptools import setup

setup(
    name='flaskr',
    packages=['flaskr'],
    include_package_data=True,
    install_requires=[
        'flask',
    ],
)
```
当使用setuptools的时候，在**MANIFEST.in**文件内声明一些需要包含的特殊文件也是必须的。
在这样的情形下，**static**和**templates**文件夹应该被包含，还用**schema.sql**文件。
创建**MANIFEST.in**文件，写入如下内容：
```python
graft flaskr/templates
graft flaskr/static
include flaskr/schema.sql
```
为了简化定位应用程序，向**flaskr/__init__.py**中添加如下代码：
```python
from .flaskr import app
```

此import语句将应用程序实例带入应用程序包的顶层,当运行应用程序的时候，Flask开发
服务器需要知道应用程序实例的位置。次import语句简化了位置过程。如果没有这个声明的话，
接下来的输出声明将会变为**export FLASK_APP=flaskr.flaskr**.

这个时候，你应该能够按照应用程序。像平时一样，我们推荐在[virtualenv](https://virtualenv.pypa.io/)
中安装，使用如下的命令进行安装：
```python
pip install --editable

pip install -e D:\Python\flask\my_flaskr
```
上面的安装语句假设是运行在项目的根目录下的，**flaskr/**。
可编辑标志允许编辑源代码，而无需在每次进行更改时重新安装Flask应用程序。
flaskr现在已经安装在你的virtualenv中了，使用**pip freeze**进行查看。
有了这些，现在你可以启动应用程序了。使用下面的命令：
```python
export FLASK_APP=flaskr
export FLASK_DEBUG=true
flask run
```
如果你使用的是Windows操作系统，你需要用 _set_ 代替 _export_ .
**FLASK_DEBUG**标志启用或禁用交互式调试器。不要在生产系统中启动调试模式，因为它将允许用户在服务器上执行代码！

您将看到一条消息，告诉您服务器,还有一个您可以访问的地址。
当您浏览浏览器中的服务器时，您将收到404错误，因为我们还没有任何视图。稍后会解决这个问题，但首先应该让数据库工作。

###外部可访问的服务器
想让你的服务器变成公用的吗？相关信息，请查看[externally visible server](http://flask.pocoo.org/docs/0.12/quickstart/#public-server)

接下来[步骤四：数据库连接](http://flask.pocoo.org/docs/0.12/tutorial/dbcon/#tutorial-dbcon)
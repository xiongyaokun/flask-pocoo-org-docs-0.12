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
  - 上一篇[步骤一：数据库方案](http://flask.pocoo.org/docs/0.12/tutorial/schema/)
  - 下一篇[步骤三：以包的形式安装flaskr](http://flask.pocoo.org/docs/0.12/tutorial/packaging/)
  
  
  
##步骤二：初始化应用程序

现在已经有数据库方案了，所以接下来你可以创建程序模块了，**flask.py**。这个文件需要放置在**flask/flask**目录下，
在模块开始的几行是很重的必需的声明，之后是几行配置代码。对于像flaskr一样的小型应用来说，可以把配置
代码放在这同一个模块中。但是一个更明朗的解决方案是，单独创建一个 _.init_ 或者 _.py_ 文件，下载，然后从它引入值。

如下的是引入声明（在**flaskr.py**文件中）：
```python
# all the imports
import os
import sqlite3
from flask import Flask, request, session, g, redirect, url_for, abort,\
    render_template, flash
```
接下来的几行代码将会创建实例，并用同一个文件里的配置初始化它**flaskr.py**:
```python
app = Flask(__name__) #create the application instance :)
app.config.from_object(__name__) #load config from this file, flaskr.py

#load default config and override config from an environment variable
app.config.update(dict(
    DATABASE = os.path.join(app.root_path, 'flaskr.db'),
    SECRET_KEY = 'development key',
    USERNAME = 'admin',
    PASSWORD = 'default'
))
app.config.from_envvar('FLASKR_SETTINGS', silent=True)
```

**Config**对象，类似一个字典，所以可以使用新值替换。

###数据库路径

操作系统知道当前进程的路径，不幸的是，对于web应用程序来时，不能这样认为，因为在一个进程中也许有多个程序在运行。

鉴于此，_app.root_path_ 属性可以被用来获取应用程的路径。配合 _os.path_ 使用，可以很轻易地找到文件。
在这个例子中，我们把数据库文件就放在与它同级目录旁边。

对于一个真的应用来说，建议使用[实例文件夹](http://flask.pocoo.org/docs/0.12/config/#instance-folders)

通常来说，加载一个独立的，环境配置文件是比较好的。Flask允许你引入多种不同的配置，它会使用你最后一次的引入的定义。
强大的配置函数setups.**from_envvar()**可以帮助这样实现。
```python
app.config.from_envvar('FLASKR_SETTINGS', silent=True)
```
简单的定义一个环境变量**FLASKR_SETTINGS**,使它指向一个被加载的配置文件，_silent_ 参数用于告诉Flask，如果没有这样的一个
环境变量的话，也可以正常运行。

此外，你也可以使用**from_object()**方法，提供一个引入的模块名。Flask将会使用这个模块来初始化变量。有一点请记住，在任何
情况下，只有大写的变量名才会被使用。

**SECRET_KEY**对于保证客户端的会话安全是必要的。请明智的选择这个值，确保它很难猜到，而且足够复杂。

最后，你要添加一个方法，使程序可以容易的链接到特定的数据库。
这可以用于根据请求以及交互式Python shell或脚本打开链接。以后，我们会做这些。
你可以通过SQLite创建一个简单的链接，并且告诉它使用**SQLite3.Row**对象展示数据表中的行。这可以使他们像字典一样，而不是元组。

```python
def connect_db():
    """Connects to the specific database."""
    rv = sqlite3.connect(app.config['DATABASE'])
    rv.row_factory = sqlite3.Row
    return rv
```

在下一部分，你将会看到如何运行程序。

请看下一步：[步骤三：以包的形式安装flaskr](http://flask.pocoo.org/docs/0.12/tutorial/packaging/#tutorial-packaging)
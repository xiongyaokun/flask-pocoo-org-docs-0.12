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
[文档预览](http://flask.pocoo.org/docs/0.12/)
- [Tutorial](http://flask.pocoo.org/docs/0.12/tutorial/)
  - 上一篇[步骤三：以包的形式安装flaskr](http://flask.pocoo.org/docs/0.12/tutorial/packaging/)
  - 下一篇[步骤五：创建数据库](http://flask.pocoo.org/docs/0.12/tutorial/dbinit/)
  
  
## 步骤四：数据库连接

现在你有一个建立数据库连接的函数 _connect_db_ ,但是，如果只靠它的话，还不是特别有用。
如果不停的打开和关闭数据库连接，是很低效的。所以，需要对连接进行一定的保存时间。
由于数据库连接封装事务，您需要确保一次只有一个请求使用连接。
一个优雅的方式是利用应用程序的上下文。

Flask提供两种上下文：**application context** 和 **request context**.
目前来说，你必须知道的是有特殊的变量使用这些。
例如，变量**request**是和当前request相关联的request对象，而**g**是应用程序上下文的一个全局变量。
这篇教程的后面会详细介绍这些。

目前来说，你需要知道的是你可以安全的使用**g**对象来存储信息。

那么，你什么时候使用呢？要做到这一点，你可以做一个帮助功能。
第一次调用该函数时，它将为当前上下文创建数据库连接，并且连续的调用将返回已建立的连接：
```python
def get_db():
    """Opens a new database connection if there is none yet for the current
    application context.
    """
    if not hasattr(g, 'sqlite_db'):
        g.sqlite_db = connect_db()
    return g.sqlite_db
```
现在你已经知道怎么建立连接了，但是怎么样以合适的方式断开连接呢？
为此，Flask为我们提供了一个名为**teardown_appcontext()**的装饰器，每次应用程序上下文关闭的
时候它都会被执行。
```python
@app.teardown_appcontext
def close_db(error):
    """Closes the database again at the end of the request."""
    if hasattr(g, 'sqlite_db'):
        g.sqlite_db.close()
```
使用**teardown_appcontext**标识的函数，每次当应用程序上下文关闭的时候，函数都会被执行，这是神马意思？
实际上，在请求之前，程序上下文已经被创建了，请求完成时，上下文就被销毁了。两个原因可以引发上下文销毁：
一切顺利（错误参数将为None）或发生异常，在这种情况下，错误将传递给销毁函数。

是不是好奇这些上下文是神马意思？请查阅文档[The Application Context](http://flask.pocoo.org/docs/0.12/appcontext/#app-context),获取更多。

继续阅读：[步骤五：创建数据库](http://flask.pocoo.org/docs/0.12/tutorial/dbinit/#tutorial-dbinit)

### 提示：
把这些代码放在哪里？
如果您一直遵循本教程，您可能会想知道从该步骤和下一步将代码放在哪里。
一个合乎逻辑的地方是将这些模块级功能分组在一起，
把 _get_db_ 和 _close_db_ 函数放在你的 _connect_db_ 函数下面。

If you need a moment to find your bearings, take a look at how the [example source](https://github.com/pallets/flask/tree/master/examples/flaskr/) is organized. 
In Flask, you can put all of your application code into a single Python module. You don’t have to, and if your app [grows larger](http://flask.pocoo.org/docs/0.12/patterns/packages/#larger-applications), it’s a good idea not to.


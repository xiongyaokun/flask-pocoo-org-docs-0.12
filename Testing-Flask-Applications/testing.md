## Flask应用测试

**Something that is untested is broken.**
上面这句话并不完全正确，但是距离事实也不远了，虽然不知道这句话从哪儿来的。
未经测试的应用，很难去改善它的代码，未经测试的应用程序的开发人员往往变得非常偏执。
如果应用程序具有自动测试功能，您可以安全地进行更改，并立即知道是否有任何故障。


Flask提供了一种通过暴露Werkzeug测试客户端，并为您处理上下文本地来测试应用程序的方法。
你可以使用它搭建自己喜欢的测试方案。在这篇文档里，我们使用Python自带的**unittest**包。

### 应用程序
首先，我们需要有一个待测试的应用程序，我们使用在[Tutorial](http://flask.pocoo.org/docs/0.12/tutorial/#tutorial)
教程中编写的程序。如果你还没有这个程序，请从这里下载[the examples](https://github.com/pallets/flask/tree/master/examples/flaskr/)

### 测试骨架
为了便于测试，我们增加第二个模块**flaskr_tests.py**，并且创建一个单元测试的骨架:
```python
import os
import flaskr
import unittest
import tempfile

class FlaskrTestCase(unittest.TestCase):
    def setUp(self):
        self.db_fd, flaskr.app.config['DATABASE'] = tempfile.mkstemp()
        flaskr.app.testing = True
        self.app = flaskr.app.test_client()
        with flaskr.app.app_context():
            flaskr.init_db()
            
    def tearDown(self):
        os.close(self.db_fd)
        os.unlink(flaskr.app.config['DATABASE'])
        
if __name__ == '__main__':
    unittest.main()
```

在**setUp()**中的代码创建了一新的测试客户端，并且初始化了一个数据库。
在每个独立的测试程序运行之前，这个函数就被执行了。为了在测试之后删除数据库，我们在**tearDown()**
方法中关闭文件，并且移除。此外，在建立的时候，**TESTING**配置旗杆也被激活了。它所做的就是阻止
请求处理时候的错误捕捉，使你在执行测试请求的时候可以得到更好的错误报告。

这个测试端口将会为我们提供一个简单的程序接口。我们可以触发对程序的请求，并且端口也会为我们跟踪cookies。

因为SQLite3是基于文件系统的，我们可以轻易的使用tempfile模块，去创建一个临时的数据库并进行初始化。
**mkstemp()**函数为我们做了两件事：它返回一个底层的文件句柄，和一个随机的文件名，这个名字是我们将要用来创建数据库的名字。
我们必须始终保存 _db_fd_ 以便于我们可以使用**os.close()**函数关闭文件。

如果现在我们运行测试的话，我们将会看到如下输出：
```python
python flaskr_tests.py
------------------------------------------------
Ran 0 tests in 0.000s

OK
```
即使这没有做任何真正的测试，我们也已经知道了这个flaskr应用是语法上有效的，否则，这样的引入会产生异常而终止。

###第一个测试

现在是时候开始测试应用程序的功能了．现在我们测试，如果我们进入应用程序的根目录，就会显示“No entries here so far”
因此，我们向类中增加一个方法，如下：
```python
class FlaskrTestCase(unittest.TestCase):

    def setUp(self):
        self.db_fd, flaskr.app.config['DATABASE'] = tempfile.mkstemp()
        flaskr.app.testing = True
        self.app = flaskr.app.test_client()
        with flaskr.app.app_context():
            flaskr.init_db()

    def tearDown(self):
        os.close(self.db_fd)
        os.unlink(flaskr.app.config['DATABASE'])

    def test_empty_db(self):
        rv = self.app.get('/')
        assert b'No entries here so far' in rv.data
```
请注意，我们的测试功能以测试文字开始;这允许unittest自动将方法识别为运行测试。
通过使用**self.app.get('/')**我们可以用特定的路径，向应用程序发送一个HTTP请求。
返回值是一个**response_class**对象。现在我们可以使用**data**属性，检验从应用程序的返回值（字符串）。
在这种情况下，我们确保‘No entries here so far’是输出的一部分。

再次运行它，你将会看到显示通过测试：
```python
python flaskr_tests.py
------------------------------------
Ran 1 test in 0.034s

OK
```

### 登陆和退出

我们应用的大部分只对于管理员才有用，所以我们需要一种方式，让我们测试的客户可以登录、推出。
为了这样做，我们使用所需的表单数据（用户名和密码）将一些请求发送到登录和注销页面。
并且因为登录和注销页面重定向，我们告诉客户follow_redirects。

向FlaskrTestCase类增加如下两个方法：
```python
def login(self, username, password):
    return self.app.post('/login', data=dict(
        username = username,
        password = password
    ), follow_redirects=True)
    
def logout(self):
    return self.app.get('/logout', follow_redirects=True)
    
```
现在我们可以轻松地测试登陆和退出的功能，并且，对于无用的数据，会显示失败。
现在把新的测试添加到类中：
```python
def test_login_logout(self):
    rv = self.login('admin', 'default')
    assert b'You were logged in' in rv.data
    rv = self.logout()
    assert b'You were logged out' in rv.data
    rv = self.login('adminx', 'default')
    assert b'Invalid username' in rv.data
    rv = self.login('admin', 'defaultx')
    assert b'Invalid passward' in rv.data
```

### 添加消息测试

我们还应该测试添加消息是否有效。像下面这样添加一个测试方法：
```python
def test_message(self):
    self.login('admin', 'default')
    rv = self.app.post('/add', data=dict(
        title = '<Hello>',
        text = '<strong>HTML</strong> allowed here'
    ), follow_redirects=True)
    assert b'No entries here so far' not in rv.data
    assert b'&lt; Hello&gt;' in rv.data
    assert b'<strong>HTML</strong> allowed here' in rv.data
```
这里我们测试HTML可以存在于文本中，但是不能在标题中，这是intended behavior.
运行测试，我们得到3个测试成功的结果：
```python
python flaskr_tests.py
...
---------------------------------
Ran 3 tests in 0.332s

OK
```
如果想测试更多关于返回头，和返回状态，请查看[MiniTwit Example](https://github.com/pallets/flask/tree/master/examples/minitwit/)
包含了许多测试套件。

### 其他一些测试技巧

除了上面介绍的用户测试以外，还有另外一种测试方法，使用**test_request_context()**方法和**with**语句
一起进行测试，以适当的方法激活请求。这种方法可以使你获得**request**,**g**,**session**对象，就像在试图函数中
一样。这是一个充分演示这种方法的例子：
```python

```
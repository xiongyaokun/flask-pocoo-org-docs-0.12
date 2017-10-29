## 快速启动

是不是很渴望马上开始啊？这篇文档将会很好的向你介绍Flask。假设你已经安装好了Flask。如果还没有安装的话，
请查看[Installation](http://flask.pocoo.org/docs/0.12/installation/#installation)部分。

### 一个小型的应用
一个很小型的Flask应用是像下面这样的：
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'
```

上面的代码做了什么呢？

    1. 首先，我们引入了**Flask**类，创建这个类的一个实例，这个实例将会成为我们的WSGI应用。
    2. 接下来，我们创建了这个类的实例，第一个参数是这个应用的包名或者模块名。
        如果你只是使用了一个模块（就像这个例子一样），你应该使用**__name__**作为参数，
        因为根据模块是作为程序运行，或者是作为包被引入，它的名字是变化的（'__main__' versus the actual import name），
        这个是必须的，因为Flask由此知道去哪里寻找模板、静态文件等等。更多信息请查看[Flask](http://flask.pocoo.org/docs/0.12/api/#flask.Flask)
    3. 我们使用**route()**装饰器去告诉Flask，什么样的URL触发哪一个函数。
    4. 该函数的给出一个名称，也生成该特定函数的URL，并返回我们要在用户浏览器中显示的消息。    
   
以 _hello.py_ 保存这个文件,或者其他类似的名字也可以，确保它的文件名不是 _flask.py_ ,因为这样会和Flask自己产生冲突。

为了运行这个程序，一种方法是使用**flask**命令，另一种方法是python's **-m** switch with Flask.
在这之前，你需要设置环境变量**FLASK_APP**,以此来告诉你的终端需要运行的程序是哪个。

    export FLASK_APP = hello.py
    flask run
    * Running on http://127.0.0.1:5000/
    
如果你的事Windows系统，你需要使用**set**代替**export**。

另一种方法，你也可以使用**python -m flask**:

    export FLASK_APP = hello.py
    python -m flask run
    * Running on http://127.0.0.1:5000/
    
这样就建立了一个很简单的内置服务器，足以测试使用了，但是不能用于生产环境。
关于部署相关的信息，请查看[Deployment Options](http://flask.pocoo.org/docs/0.12/deploying/#deployment)

现在请转向[http://127.0.0.1:5000/](http://127.0.0.1:5000/),你将会看到自己的hello world问候！

### 外部可访问的服务器

如果你运行上面的服务，你会发现仅仅你自己的电脑可以访问这个服务器，互联网上的其他电脑不能访问。
这是默认值，因为在调试模式下，应用程序的用户可以在计算机上执行任意Python代码。

如果你的调试模式是关闭的，或者你相信你网络内的用户，你可以通过如下代码使服务器在远程网络内可见：

    flask run --host=0.0.0.0
    
这将会告诉你的系统，监听所有公用的IP地址。

### 如果服务没有启动怎么办？

如果**python -m flask**失败，或者**flask**不存在，可能会有多种原因导致的。
首先，你需要查看错误信息。

#### 旧版本的Flask

在0.11版本之前的Flask，会有不同的启动应用的方式。简单地说，**flask**命令行不存在，
**python -m flask**也一样不存在。在这样的情况下，你有两种选择：一种是升级到新版本的Flask，
另一种是查看[Development Server](http://flask.pocoo.org/docs/0.12/server/#server)
文档，选择一个可用的启动方法。

### 无效的导入名称

**FLASK_APP**环境变量是在**flask run**的时候，需要导入的模块名称。
如果模块名称不正确，您将在启动时收到导入错误（或者当您导航到应用程序时启用调试）。
它会告诉你它在尝试引入什么，为什么发生错误了。

最常见的错误是打字错误，或者没有创建应用对象。

### 调试模式

（想要记录错误和堆栈跟踪？请参阅[Applications Errors](http://flask.pocoo.org/docs/0.12/errorhandling/#application-errors)）

**flask**脚本可以很好的启动一个本地的开发环境，但是当你的代码有改动的时候，你必须手动重新启动服务器。
这样就感觉不方便，Flask可以做的更好。

如果启用了调试支持，代码更改以后服务器将自动重新载入代码，如果出现问题，它还将为您提供一个有用的调试器。

你可以在运行前，通过设置**FLASK_APP**环境变量来启动调试模式。

    $ export FLASK_APP=1
    $ flask run
    
(在Windows环境下，需要使用**set**代替**export**)

上面代码做了如下事情：

1. 激活了调试器；
2. 激活了自动重新加载；
3. 打开了Flask应用的调试模式。

在[Development Server](http://flask.pocoo.org/docs/0.12/server/#server)文档中详细解释了更多的参数。

### 注意

即使交互式调试器在分支环境中不起作用（这使得几乎不可能在生产服务器上使用）。
它仍然允许执行任意代码。这使得它成为主要的安全风险，因此绝不能在生产环境上使用。

下面是一个调试器的截图：
![image](http://flask.pocoo.org/docs/0.12/_images/debugger.png)

Hava another debugger in ming? see [Working with Debubbers](http://flask.pocoo.org/docs/0.12/errorhandling/#working-with-debuggers)


### 路由

现代的移动应用程序都有美观的URL地址。这便于人们记忆，这对于使用较慢网络连接的移动设备的应用来说尤其如此。
如果用户可以直接进入所需的页面，而不必打索引页面，则更有可能他们会喜欢该页面，并且下次再来。

就像你在上面看见的，路由装饰器**route()**的作用就是把一个函数绑定到一个URL地址。
下面是一些基本的例子：

```python
@app.route('/')
def index():
    return 'Index Page'
    
@app.route('/hello')
def hello():
    return 'Hello, World!'
```
当然，不仅仅如此！您可以使URL的某些部分动态，并将多个规则附加到一个函数。

### 变量规则

向URL中添加变量，你可以这样标记 **<variable_name>** 。
这个部分会以关键字的形式出入到你的函数中，你也可以选择使用类型声明 **<converter:variable_name>**
。下面是一些比较好的例子：
```python
@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return 'User %s' % username
    
@app.route('/post/<int:post_id>')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return 'Post %d' % post_id
```
有以下的类型转换：

    |name    | descriptions                                   |
    |:-------|-----------------------------------------------:|
    |string  | accepts any text without a shash (the default) |
    |int     | accepts integers                               |
    |float   | like _int_ but for floating point values       |
    |path    | like the default but also accepts slashes      |
    |any     | mathches one of the items provided             |
    |uuid    | accepts UUID strings                      |
    
### 特殊的URL/重定向行为

Flask的URL规则基于Werkzeug的路由模块。他的这个模块背后的想法是确保基于Apache和早期的HTTP服务器规定的先例的漂亮而唯一的URL。

看一下下面两个规则：
```python
@app.route('/projects/')
def projects():
    return 'The project page'
    
@app.route('/about')
def about():
    return 'The about page'
```
虽然他们看起来很像，但是区别在于URL定义时使用的斜杠 **/** 。在第一个URL定义中，
**projects**地址的末尾有一个 **/** 。这就类似于文件系统中的一个文件夹。
当你尝把斜杠去掉的时候，它会重定向到带斜杠的URL。

在第二个示例中，URL末尾是没有斜杠的，而是像类UNIX系统上的文件的路径名。使用尾部斜杠访问该URL将产生404“未找到”错误。

这种行为允许相对URL继续工作，即使省略尾部斜线，与Apache和其他服务器的工作原理一致。
此外，URL将保持唯一，这有助于搜索引擎避免对同一页面进行两次索引。

### 构建URL

如果可以匹配URL，是否Flask也可以生成它们？回答是肯定的。你可以使用**url_for()**函数把一个URL绑定到特定的
视图函数。它允许函数名作为第一个参数，和其他的一些关键字参数，每一个对应于URL中的可变部分。
未知的变量部分作为查询参数附加到URL。下面是一些例子：
```python
from flask import Flask, url_for
app = Flask(__name__)

@app.route('/')
def index(): pass

@app.route('/login')
def login(): pass

@app.route('/user/<username>')
def profile(username): pass

with app.test_request_context():
    print (url_for('index'))
    print (url_for('login'))
    print (url_for('login', next='/'))
    print (url_for('profile', username='John Doe'))
```
输出如下：
    
    /
    /login
    /login?next=/
    /user/John%20Doe
    
这里使用**test_request_context()**方法，解释如下。它告诉Flask像处理请求一样，
即使我们使用的是Python shell。请查看详细描述[Context Locals](http://flask.pocoo.org/docs/0.12/quickstart/#context-locals)

为什么要使用URL构建函数 **url_for()** 来构建URL，而不是将它们直接写到模板中？
这样做有3个好处：
1. Reversing通常比直接硬编码更具有可读性。更重要的是，它允许您一次更改URL，而无需记住所有需要更改URL。
2. URL构建将为您透明地处理特殊字符和Unicode数据的转义，因此您不必处理它们。
3. 如果您的应用程序位于URL根目录之外 - 例如，**/myapplication** 而不是 **/** , **url_for()** 将会很好的处理他们。

### HTTP方法

HTTP（Web应用程序直间进行沟通的协议）知道用于访问URL的不同方法。默认的，一个路由仅仅对**GET**请求进行回应。
但是你可以通过向**route()**装饰器增加**methods**参数来改变。下面是一些示例：

```python
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        do_the_login()
    else:
        show_the_login_form()
```
如果是**GET**请求，**HEAD**会自动为你加上，你不用自己进行处理。也会确保请求头**HEAD**按照[HTTP RFC](http://www.ietf.org/rfc/rfc2068.txt)（the document describing the HTTP protocol）
的要求进行处理。和Flask0.6一样，**OPTIONS**也会自动为你实现。

你是不是不知道什么是HTTP方法啊？不必担心，这里有一个HTTP方法的快速介绍，并且解释为什么它们很重要：

#### GET
    浏览器告诉服务器只需获取存储在该页面上的信息并发送它。这可能是最常用的方法。
    
#### HEAD

浏览器告诉服务器获取信息，但它只对页眉感兴趣，而不是页面的内容。一个应用程序应该可以处理这样的请求，就像接到一个**GET**请求，
但是不返回实际的内容。在Flask中，你一点儿也不需要为此担心，底层的Werkzeug库会为你做这些。

#### POST

浏览器告诉服务器，它想向URL中推送一些新的信息，并且服务器需要保证数据被保存了，而且只保存了一次。
这是一种HTML表单(forms)经常向服务器传送数据的方式。

#### PUT

和**POST**相似，但服务器可能会多次触发存储过程来覆盖旧值。也许你会疑问，为什么需要这个呢？但是这样做确实有一些好处。
假如在传输数据的时候连接断开了，在这种情况下，浏览器和服务器之间的系统可能会第二次安全地接收请求，而不会破坏事件。
如果使用的是**POST**请求的话，就没法实现，因为只会被触发一次。

#### DELETE

删除在特定位置的信息。

#### OPTIONS

为客户提供一个快速的方法来确定此URL支持哪些方法。从Flask 0.6开始，这是为您自动实现的。

### 静态文件

动态的互联网应用也需要静态的文件。通常也是存储CSS和JavaScript文件的地方。
理想情况下，您的Web服务器被配置为为您服务，但在开发期间，Flask也可以执行此操作。
只需在程序包中或在模块旁边创建一个名为**static**的文件夹，它将在应用程序的 **/static** 处可用。
如果为静态文件创建一个URL地址，使用特殊的**static**端点名称：
```python
url_for('static', filename='style.css')
```
这个文件必须被保存在目录**static/style.css**的文件系统下。

### 模板渲染

在Python中生成HTML不是很有趣的事情，实际上很麻烦，因为您必须自行执行HTML转义以保护应用程序的安全。
鉴于此，Flask默认使用[Jinjia2](http://jinja.pocoo.org/)模板引擎。

你可以使用**render_template()**方法渲染一个模板。你所需要做的仅仅是提供一个模板的名字以及想要传入模板的一组变量名。
这组变量名是以字典的形式传入的。下面是一个简单的示例，如何渲染模板：
```python
from flask import render_template

@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)
```

Flask将会在**templates**文件夹内寻找模板。所以，如果你的程序是一个模块的话，该文件夹位于模块的旁边，
如果他是一个包的话，该文件夹位于包内。

#### 示例1：模块
    /application.py
    /templates
        /hello.html
        
#### 示例2：包
    /application
        /__init__.py
        /templates
            /hello.html
            
对于模板，你可以使用Jinjia2模板的全部功能。请到官网[Jinjia2 Template Documentation](http://jinja.pocoo.org/docs/templates)
获取更多信息。

下面是一个示例模板：
```html
<!doctype html>
<title>Hello from Flask</title>
{% if name %}
    <h1>Hello {{name}}</h1>
{%else%}
    <h1>Hello, World!</h1>
{%endif%}
```

在模板内部，你也可以使用**request, session**和**g**对象，还有**get_flashed_messages()**函数。

在模板中使用继承很重要，如果你想知道继承是怎么工作的，请点击[Template Inheritance](http://flask.pocoo.org/docs/0.12/patterns/templateinheritance/#template-inheritance)
文档。基本的模板继承可以使每一个页面有相同的标题、导航、页脚。
模板具有自动转译的功能，如果**name**内板涵HTML内容，那么它将会被自动转译。如果你相信一个变量（例如因为它来自将wiki标记转换为HTML的模块），并且你知道它是安全的
HTML内容，那么你可以使用**Markup**类进行标记，或者你也可以在模板内使用 **|safe** 过滤器。
请到Jinjia2文档获得更多信息。

下面简单介绍**Markup**类如何工作的：
```python
from flask import Markup

Markup('<strong>Hello %s!</strong>') % '<blink>hacker</blink>'
# Markup(u'<strong>Hello &lt;blink&gt;hacker&lt;/blink&gt;!</strong>')

Markup.escape('<blink>hacker</blink>')
# Markup(u'&lt;blink&gt;hacker&lt;/blink&gt;')

Markup('<em>Marked up</em> &raquo; HTML').striptags()
# u'Marked up \xbb HTML'
```
版本0.5的变化：所有模板不再启用自动转译。模板的以下扩展名触发自动转义：.html，.htm，.xml，.xhtml。从字符串加载的模板将禁用自动转义。

### 获取请求数据

对于Web应用程序，对客户端发送到服务器的数据做出反应至关重要。在Flask中,这些信息由全局变量**request**对象提供。
如果你对Python有了解，也许会有疑问，为什么它可以是全局的（因为Python没有声明的全局、局部变量），并且Flask怎么
保证的线程安全。答案书本地上下文环境(context locals)：

### 本地上下文(context locals)

#### 内幕消息(Insider Information)
如果您想了解这些工作原理以及如何使用上下文本地实现测试，请阅读本节，否则只需跳过该部分。

在Flask内有一些固定的全局对象，但是不是常规所说的那样的全局对象。
这些对象实际上是对特定上下文对象的哦代理。太拗口了！！！但实际上这很容易理解。

想象一下一个正在处理线程的上下文，这时一个新的请求进来了，web服务器觉定新开一个线程（或其他的方式，底层对象能够处理除线程之外的并发系统），
当Flask启动其内部请求处理时，它会找出当前线程活动的上下文，并将当前应用程序和WSGI环境绑定到该上下文（线程）。
它以一种智能的方式执行，以便一个应用程序可以调用另一个应用程序而不会中断。

所以，这对于你来说意味着什么？基本上你完全可以忽视，除非你在做某些事情，如单元测试的时候。您将注意到，由于没有请求对象，依赖于请求对象的代码将突然中断。
解决方案是自己创建一个请求对象并将其绑定到上下文。对于单元测试来说，最简单的方法是使用**test_request_context()**方法。
结合with语句，它将绑定一个测试请求，以便您可以与它进行交互。下面是一个示例：

```python
from flask import request

with app.test_request_context('/hello', method='POST'):
    # now you can do something with the request until the 
    # end of the with block, such as basic assertions:
    assert request.path == '/hello'
    assert request.method == 'POST'
    
```

另一种可能性是将整个WSGI环境传递给 **request_context()** 方法：
```python
from flask import request
with app.request_context(environ):
    assert request.method == 'POST'
```

### 请求对象(Request Object)

请求对象在API部分中有讲述，我们将不在此详细介绍，参加[request](http://flask.pocoo.org/docs/0.12/api/#flask.request)
这是一些最常见的操作的概述。首先，你需要从 _flask_ 模块中引入:
```python
from flask import request, render_template
```
使用**method**属性，可以获取当前的请求方法，你可以使用**form**属性获取表单数据（以 _POST_ 和 _PUT_ 请求发送的数据）
以下是上述两个属性的完整示例：
```python
from flask import request
@app.route('/login', methods=['POST', 'GET'])
def login():
    error = None
    if request.method == 'POST':
        if valid_login(request.form['username'], request.form['password']):
            return log_the_user_in(request.form['username'])
        else:
            error = 'Invalid username/password'
        # the code below is executed if the request mentod
        # was GET or the credentials were invalid
        return render_template('login.html', error=error)
```

如果 _form_ 属性中没有所寻找的关键字，将会发生什么？在这种情况下，会抛出一个[KeyError](https://docs.python.org/3/library/exceptions.html#KeyError)错误。
你可以像捕捉标准错误一样对它进行捕获，如果你没有捕获的话，则会出现一个 _HTTP404_ 错误请求页面。
所以在许多情况下，你不必处理这个问题。

如果想获取在UTL(?key=value)中提交的参数，你可以使用**args**属性：
```python
from flask import request
searchword = request.args.get('key', '')
```
我们建议使用 _get_ 方法获取URL参数，或者通过捕获[KeyError](https://docs.python.org/3/library/exceptions.html#KeyError)
为你用户可能输入错的URL，这时候，如果返回 _400_ 的错误请求页面会显得不友好。

如果想查看**request**对象的全部方法和属性，请点击[request](http://flask.pocoo.org/docs/0.12/api/#flask.request).

### 上传文档

使用Flask，你可以轻易的处理文件的上传。请确保在你的HTML表单里设置 **enctype='multipart/form-data'** 属性。
否则，浏览器不会发送你的文件滴。

待上传的文件存储在文件系统的内存或临时位置。你可以通过查看**request**对象里的**files**属性，来获得这些文件。
每一个待上传的文件都是放在这个字典里。它就像一个标准的Python文件对象一样，但它也有一个 **save()** 方法，可以将该文件存储在服务器的文件系统上。
下面是一个简单的示例，展示它是如何工作的:
```python
from flask import request

@app.route('/upload', methods=['POST', 'GET'])
def upload_file():
    if request.method == 'POST':
        f = request.files['the_file']
        f.save('/var/www/uploads/upload_file.txt')
```
如果您想知道文件在客户端上传到应用程序之前如何命名，你可查看[filename](http://werkzeug.pocoo.org/docs/datastructures/#werkzeug.datastructures.FileStorage.filename)属性。
但是，请记住，这个值是可以被伪造的，永远不会相信这个价值。如果要使用客户端的文件名将文件存储在服务器上，请使用
Werkzeug提供给你的**secure_filemame()**函数，示例如下：
```python
from flask import request
from werkzeug import secure_filename

@app.route('/upload', methods=['POST', 'GET'])
def upload_file():
    if request.method == 'POST':
        f = request.files['the_file']
        f.save('/var/www/uploads/' + secure_filename(f.filename))
```
想看更好的例子，请查阅[Uploading Files](http://flask.pocoo.org/docs/0.12/patterns/fileuploads/#uploading-files)


### Cookies

可以使用[cookies](http://flask.pocoo.org/docs/0.12/api/#flask.Request.cookies)属性获取cookies。
可以使用response对象的[set_cookie]方法设置cookies。request对象的[cookies](http://flask.pocoo.org/docs/0.12/api/#flask.Request.cookies)属性
是一个包含所有用户传递的cookies的字典。如果你想使用sessions，，就不要直接使用cookies，请使用Flask提供的[Sessions](http://flask.pocoo.org/docs/0.12/quickstart/#sessions)
在cookies的基础上增加了安全性。

示例，读cookies:
```python
from flask import request

@app.route('/')
def index():
    username = request.cookies.get('username')
    # use cookies.get(key) instead of cookies[key] to not get a
    # KeyError if the cookies is missing.
```
示例，保存cookies：
```python
from flask import make_response, render_template

@app.route('/')
def index():
    resp = make_response(render_template(...))
    resp.set_cookie('username', 'the username')
    return resp
```

请记住，是在response对象上设置cookies。因为你通常只是从视图函数返回字符串，Flask将会将它们转换为响应对象。
如果您明确要这样做，可以使用[make_response](http://flask.pocoo.org/docs/0.12/api/#flask.make_response)函数，然后修改它。

有时您可能想要在响应对象不存在的位置设置一个cookie。这可以使用[Defered Request Callbacks](http://flask.pocoo.org/docs/0.12/patterns/deferredcallbacks/#deferred-callbacks)模式来实现。
这一部分请查阅[About Response](http://flask.pocoo.org/docs/0.12/quickstart/#about-responses)

### 重定向和错误(Redirects and Errors)

要将用户重定向到另一个端点，请使用[redirect](http://flask.pocoo.org/docs/0.12/api/#flask.redirect)函数.
要使用错误代码提前中止请求，请使用[abort](http://flask.pocoo.org/docs/0.12/api/#flask.abort)函数：
```python
from flask import abort, redirect, url_for

@app.route('/')
def index():
    return redirect(url_for('login'))
    
@app.route('/login')
def login():
    abort(401)
    this_is_never_executed()
```
这是一个相当无关的例子，因为用户将被从索引重定向到无法访问的页面(401意味着拒绝访问)，仅仅展示如何使用。


默认情况下，每个错误代码都会显示黑白错误页面。如果你想自己设置错误页面，你可以使用[errorhandler()](http://flask.pocoo.org/docs/0.12/api/#flask.Flask.errorhandler)装饰器。
```python
from flask import render_template

@app.route(404)
def page_not_found(error):
    return render_template('page_not_found.html'), 404
```

请注意在函数[render_template](http://flask.pocoo.org/docs/0.12/api/#flask.render_template)后面的 _404_ 。
这告诉Flask该页面的状态代码应该是404，这意味着没有找到。默认情况下，200意味着：一切顺利。

更多细节，请查看[Error handler](http://flask.pocoo.org/docs/0.12/errorhandling/#error-handlers).

### 关于响应

视图函数的返回值将自动转换为响应对象。如果返回值是一个字符串，则将其转换为响应对象，其中字符串作为响应体，
200 OK状态代码和text / html mimetype。Flask将返回值转换为响应对象所使用的的逻辑如下：
1. 如果返回正确类型的响应对象，则从视图直接返回。
2. 如果它是一个字符串，则使用该数据和默认参数创建一个响应对象。
3. 如果返回一个元组，元组中的项可以提供额外的信息。这样的元组形式必须是(response, status, headers) or (response, headers)，元组中必须至少有一个元素。
    状态值将覆盖状态代码，标题可以是附加标题值的列表或字典。
4. 如果以上情况都没有，则Flask将假定返回值是一个有效的WSGI应用程序，并将其转换为响应对象。

如果您想要在视图中获取生成的响应对象，你可以使用[make_response](http://flask.pocoo.org/docs/0.12/api/#flask.make_response)函数。
假设你有下面这个视图函数：
```python
@app.errorhandler(404)
def not_found(error):
    return render_template('error.html'), 404
```
您只需要使用make_response（）包装返回表达式，并获取响应对象进行修改，然后返回：
```python
@app.errorhandler(404)
def not_found(error):
    resp = make_response(render_template('error.html'), 404)
    resp.headers['X-Something'] = 'A value'
    return resp
```

### 会话(sessions)

除了请求对象之外，还有一个名为[session](http://flask.pocoo.org/docs/0.12/api/#flask.session)的第二个对象，它允许您将特定于用户的信息从一个请求存储到下一个请求。
这是在您的cookies之上实现的，并且用密码标识cookie。这意味着用户可以查看您的cookie的内容，但不修改它，除非他们知道用于签名的密钥。

为了使用会话，你必须设置一个密钥。下面展示会话是如何工作的：
```python
from flask import Flask, session, redirect, url_for, escape, request

app = Flask(__name__)

@app.route('/')
def index():
    if 'username' in session:
        return 'Logged in as %s' escape(session['username'])
    return 'You are not logged in.'
    
@app.route('/login', methods=['POST', 'GET'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))
    return '''
        <form method='post'>
            <p><input type=text name=username>
            <p><input type=submit value=login>
        </form>
    '''
    
@app.route('logout')
def logout():
    # remove the username from the session if it's there
    session.pop('username', None)
    return redirect(url_for('index'))
    
# set the secret key. keep this really secret.
app.secret_key = 'A0Zr98j/3yX R~XHH!jmN]LWX/,?RT'
```
如果您不使用模板引擎（如本例所示），这里的[escape()](http://flask.pocoo.org/docs/0.12/api/#flask.escape)会为您转义。

#### 如何生成好的密钥

随机的问题是很难判断真正的随机性。密钥应尽可能随机.您的操作系统有方法根据加密随机生成器生成漂亮的随机东西，可以用于获取这样的密钥：
```python
import os
os.urandom(24)
```
基于Cookie的会话的提示：Flask将会取得您放入会话对象的值并将其序列化，放入cookie。
如果您发现某些值不会在请求中持久存在，而你确实启用了Cookie，并且您没有收到明确的错误消息，请检查您的页面响应中的cookie大小与Web浏览器支持的大小相比。

除了默认的基于客户端的会话之外，如果要在服务器端处理会话，还有几个Flask扩展支持此功能。

### 消息闪烁(Message Flashing)

良好的应用程序和用户界面都是关于反馈的。如果用户没有得到足够的反馈，他们可能会最终讨厌应用程序。Flask提供了一种非常简单的方法来向用户提供闪烁系统的反馈。
消息闪烁使在请求结束时记录一个消息，并在下一个（而且只有下一个）请求中访问它变得简单。这通常与布局模板组合以展示消息。

使用[flash()](http://flask.pocoo.org/docs/0.12/api/#flask.flash)方法闪烁消息。
您可以使用[get_flashed_messages()](http://flask.pocoo.org/docs/0.12/api/#flask.get_flashed_messages)获取消息，这个方法也可以在模板中使用。
查阅[Message Flashing](http://flask.pocoo.org/docs/0.12/patterns/flashing/#message-flashing-pattern)获取更多示例。

### Logging

New in version 0.3。

有时您可能处于这样的情况下，你处理的数据应该是正确的，但是实际上它是错误的。
例如，您可能有一些客户端代码向服务器发送HTTP请求，但显然格式错误。这可能是由用户篡改数据或客户端代码失败引起的。
大多数情况下，在这种情况下可以回复**400 Bad Request**，但有时候不会这样做，并且代码必须继续工作。

你可能还想记录发生了什么事情。这是loggers派上用场的地方。
Flask 0.3版本，预先配置了一个logger供您使用。下面是一些示例：
```python
app.logger.debug('A value for debugging')
app.logger.warning('A warning occurred (%d apples)', 42)
app.logger.error('A error occurred')
```
这个[logger](http://flask.pocoo.org/docs/0.12/api/#flask.Flask.logger)是一个标准的logging[Logger](https://docs.python.org/3/library/logging.html#logging.Logger),
请前往[logging documentation](https://docs.python.org/library/logging.html)获取更多信息。
更多[Application Errors](http://flask.pocoo.org/docs/0.12/errorhandling/#application-errors)

### Hooking in WSGI Middlewares

如果要向应用程序添加WSGI中间件，则可以包装内部WSGI应用程序。
例如，如果要使用Werkzeug软件包中的一个中间件来处理lighttpd中的错误，可以这样做：
```python
from werkzeug.contrib.fixers import LighttpdCGIRootFix
app.wsgi_app = LighttpdCGIRootFix(app.wsgi_app)
```

### 使用Flask扩展
扩展是帮助您完成常见任务的软件包。例如，Flask-SQLAlchemy提供SQLAlchemy支持，使其易于与Flask一起使用。
更多Flask扩展，请查看[Flask Extensions](http://flask.pocoo.org/docs/0.12/extensions/#extensions)

### 部署到Web服务器
准备好部署你的新Flask应用了吗？参加[Deployment Options](http://flask.pocoo.org/docs/0.12/deploying/#deployment).
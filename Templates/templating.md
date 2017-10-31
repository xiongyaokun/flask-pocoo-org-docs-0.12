## 模板

Flask 使用 Jinja2 作为默认模板引擎。你完全可以使用其它模板引擎。但是不管你使用 哪种模板引擎，都必须安装 Jinja2 。因为使用 Jinja2 可以让 Flask 使用更多依赖于 这个模板引擎的扩展。

本文只是简单介绍如何在 Flask 中使用 Jinja2 。如果要详细了解这个模板引擎的语法， 请查阅 [Jinja2 模板官方文档](http://jinja.pocoo.org/2/documentation/templates) 。

### Jinjia设置

在Flask中，Jinjia2默认配置如下：
- 扩展名为.html、.htm、.xml、和.xhml的模板中开启自动转义。
- 在模板中可以使用 {% autoescape %} 来手动设置是否转义。
- Flask 在 Jinja2 环境中加入一些全局函数和辅助对象，以增强模板的功能。

### 标准环境

缺省情况下，以下全局变量可以在Jinjia2模板中使用。
**config**
当前配置对象(flask.config)
0.6新版功能。
在0.10版更改，此版本开始，这个变量总是可用，甚至是在被导入的模板中。

**request**

当前请求对象([flask.request](http://flask.pocoo.org/docs/0.12/api/#flask.request))
在没有激活请求环境的情况下渲染模板时，这个变量不可用。

**session**
当前会话对象（[flask.session](http://flask.pocoo.org/docs/0.12/api/#flask.session)）
在没有激活请求环境的情况下渲染模板时，这个变量不可用。

**g**
请求绑定的全局变量（[flask.g](http://flask.pocoo.org/docs/0.12/api/#flask.g)）
在没有激活请求环境的情况下渲染模板时，这个变量不可用。

### url_for()
[flask.url_for()](http://flask.pocoo.org/docs/0.12/api/#flask.url_for)函数

### get_flashed_messages()
[flask.get_flashed_messages()](http://flask.pocoo.org/docs/0.12/api/#flask.get_flashed_messages)函数

### Jinjia环境行为

这些添加到环境中的变量不是全局变量。与真正的全局变量不同的是这些变量在已导入的模板中是不可见的。这样做
是基于性能的原因，同时也考虑让代码更有条理。
那么对你来说又有什么意义呢？假设你需要导入一个宏，这个宏需要访问请求对象， 那么你有两个选择：

1. 显式地把请求或都该请求有用的属性作为参数传递给宏。
2. 导入 “with context” 宏。
导入方式如下：
```python
{% from '_helper.html' import my_macro with context %}
```

### 标准过滤器
在Flask模板中添加了以下Jinjia2本身没有的过滤器：

#### tojson()
这个函数可以把对象转换为JSON格式。如果你要动态生成JavaScript,那么这个函数非常有用。
注意，在 _script_ 标记内部不能转义，因此在Flask 0.10之前的版本中，如果要在 _script_ 标记内部
使用这个函数必须用 **|safe** 关闭转义：
```html
<script type=text/javascript>
    doSomethingWith({{ user.username|tojson|safe }});
</script>
```

### 控制自动转义

自动转义是指自动对特殊字符进行转义。特殊字符是指HTML（或XML和XHTML）
中的&,>,<,"和>,因为这些特殊字符代表了特殊的意思，所以如果要在文本中使用它们就必须把它们替换为“实体”。如果不转义，那么用户就 无法使用这些字符，而且还会带来安全问题。
参加[跨站脚本攻击（XSS）](http://flask.pocoo.org/docs/0.12/security/#xss)

但是有时候你得禁止模板中的自动转义。比如，需要将HTML文档直接插入到页面中，
如果它们来自一个安全的系统，比如，就像从markdown到HTML的转换器。

有3种方法可以实现：
- 在Python代码中，在把HTML字符串传入模板之前，使用[markup](http://flask.pocoo.org/docs/0.12/api/#flask.Markup)对象，对HTML字符串进行包装
    通常，这个是推荐的做法。
- 在模板内部，使用 **|safe** 过滤器，显式得把字符串标记为安全的HTML **{{ myvariable|safe }}**。
- 暂时禁用自动转义系统。

你可以使用 **{% autoescape %}** 语句块，在模板中禁用自动转义：
```html
{% autoescape false %}
    <p>autoescaping is disabled here</p>
    <p>{{ will_not_be_escaped }}</p>
{% endautoescape %}
```
任何时候在你这样做的时候，对于你在这个块中使用的变量，都要特别注意。

### 注册过滤器

你有两种方法可以在Jinjia2模板中注册过滤器。你可以手动把过滤器加入[jinjia_env](http://flask.pocoo.org/docs/0.12/api/#flask.Flask.jinja_env)中，
或者使用[template_filter](http://flask.pocoo.org/docs/0.12/api/#flask.Flask.template_filter)过滤器。

下面是两个例子，功能都是把对象反转：
```python
@app.template_filter('reverse')
    def reverse_filter(s):
        return s[::-1]
        
def reverse_filter(s):
    return s[::-1]
app.jinjia_env.filters['reverse'] = reverse_filter
```

在使用装饰器的时候，如果你想使用函数名作为装饰器的名字，装饰器内的参数可以不写。
注册完成以后，你就可以在模板中像内置的过滤器一样，使用新注册的过滤器。
例如，你有一个名为 _mylist_ 的列表：
```html
{% for x in mylist | reverse%}
{% endfor %}
```

### 上写文处理器

为了自动向模板上下文环境添加变量，上下文处理器就在Flask中产生了。
在模板渲染前，上下文处理器就开始运行，它可以向模板上下文中添加新的变量。
上下文处理器是一个返回字典的函数。字典中的键值对会和模板上下文结合在一起。
```python
@app.context_processor
def inject_use():
    return dict(user=g.user)
```
上面的上下文处理器在模板中产生了一个名为 _user_ 的变量，它的值是 _g.user_ .
这个例子没有太大用处，因为全局变量 _g_ 在所有模板中都是可用的。但是，这个例子展示了怎样
使用上下文处理器。

变量不是被固定的值绑定的。上下文处理器也可以使一个函数变成在模板中可用的
因为Python允许把函数作为参数进行传递：
```python
@app.context_processor
def utility_processor():
    def format_price(amount, currency=u'€'):
        return u'{0:.2f}{1}'.format(amount, currency)
    return dict(format_price=format_price)
```
上面的例子中，上下文处理器把函数 _format_price_ 变成了在所有模板中可用。

```html
{{ format_price(0.33) }}
```
你也可以把 _format_price_ 做成一个模板过滤器，参加[Registering Filters](http://flask.pocoo.org/docs/0.12/templating/#registering-filters)
上面的例子仅仅是为了展示怎样在上下文处理器中传入函数对象。
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
  - 上一篇[步骤零：创建文件夹](http://flask.pocoo.org/docs/0.12/tutorial/folders/)
  - 下一篇[步骤二：创建应用程序](http://flask.pocoo.org/docs/0.12/tutorial/setup/)

## 步骤一：数据库方案

在这一步骤中，你将会创建一个数据库方案，这个应用只需要一张表，仅支持SQLite，你需要做的就是在**flask/flask**
文件夹下创建一个名为**schema.sql**的文件，文件内包含如下代码：
```
drop table if exits entries;
create table entries(
    id integer primary key autoincrement,
    title text not null,
    'text' text not null
);
```
这个方案只包含了一张名为**entries**的表，这张表的每一行都有一个id，一个title,一个text。id是一个主键，
自动增加，另外两个是文本，而且不能为空。

开始下一步：[步骤二：初始化应用](http://flask.pocoo.org/docs/0.12/tutorial/setup/#tutorial-setup)
---
layout: post
title:  "【个人总结】Python Flask总结"
categories:  Python Flask
tags:  总结
---

* content
{:toc}


# Python Flask Web 框架入门

笔者来总结一下最近学习Flask的一些知识

## 1. 安装

通过pip3安装Flask即可：

```shell
$ sudo pip3 install Flask
```

进入python交互模式看下Flask的介绍和版本：

```shell
$ python3

&gt;&gt;&gt; import flask
&gt;&gt;&gt; print(flask.__doc__)

    flask
    ~~~~~

    A microframework based on Werkzeug.  It's extensively documented
    and follows best practice patterns.

    :copyright: © 2010 by the Pallets team.
    :license: BSD, see LICENSE for more details.

&gt;&gt;&gt; print(flask.__version__)
1.0.2

```

## 2. 从 Hello World 开始

使用Flask写一个显示”Hello World!”的web程序

### 2.1 Hello World

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

`static`和`templates`目录是默认配置，其中`static`用来存放静态资源，例如图片、js、css文件等。`templates`存放模板文件。 我们的网站逻辑基本在`server.py`文件中，当然，也可以给这个文件起其他的名字。

在`server.py`中加入以下内容：

```xpath
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```

运行`server.py`：

```shell
$ python3 server.py 
 * Running on http://127.0.0.1:5000/
```

打开浏览器访问`http://127.0.0.1:5000/`，浏览页面上将出现`Hello World!`。 终端里会显示下面的信息：

```shell
127.0.0.1 - - [16/May/2014 10:29:08] "GET / HTTP/1.1" 200 -
```

变量app是一个Flask实例，通过下面的方式：

```xpath
@app.route('/')
def hello_world():
    return 'Hello World!'
```

当客户端访问`/`时，将响应`hello_world()`函数返回的内容。注意，这不是返回`Hello World!`这么简单，`Hello World!`只是HTTP响应报文的实体部分，状态码等信息既可以由Flask自动处理，也可以通过编程来制定。

### 2.2 修改Flask的配置

```xpath
app = Flask(__name__)
```

上面的代码中，python内置变量`__name__`的值是字符串`__main__`。Flask类将这个参数作为程序名称。当然这个是可以自定义的，比如`app = Flask("my-app")`。

Flask默认使用`static`目录存放静态资源，`templates`目录存放模板，这是可以通过设置参数更改的：

```xpath
app = Flask("my-app", static_folder="path1", template_folder="path2")

```

更多参数请参考`__doc__`：

```xpath
from flask import Flask
print(Flask.__doc__)

```

### 2.3 调试模式

上面的server.py中以`app.run()`方式运行，这种方式下，如果服务器端出现错误是不会在客户端显示的。但是在开发环境中，显示错误信息是很有必要的，要显示错误信息，应该以下面的方式运行Flask：

```xpath
app.run(debug=True)

```

将`debug`设置为`True`的另一个好处是，程序启动后，会自动检测源码是否发生变化，若有变化则自动重启程序。这可以帮我们省下很多时间。

### 2.4 绑定IP和端口

默认情况下，Flask绑定IP为`127.0.0.1`，端口为`5000`。我们也可以通过下面的方式自定义：

```xpath
app.run(host='0.0.0.0', port=80, debug=True)

```

`0.0.0.0`代表电脑所有的IP。`80`是HTTP网站服务的默认端口。什么是默认？比如，我们访问网站`http://www.example.com`，其实是访问的`http://www.example.com:80`，只不过`:80`可以省略不写。

由于绑定了80端口，需要使用root权限运行`server.py`。也就是：

```xpath
$ sudo python3 server.py

```



## 3. 获取 URL 参数

URL参数是出现在url中的键值对，例如`http://127.0.0.1:5000/?disp=3`中的url参数是`{'disp':3}`。

### 3.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 3.2 列出所有的url参数

在server.py中添加以下内容：

```xpath
from flask import Flask, request

app = Flask(__name__)


@app.route('/')
def hello_world():
    return request.args.__str__()


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

在浏览器中访问`http://127.0.0.1:5000/?user=Flask&amp;time&amp;p=7&amp;p=8`，将显示：

```xpath
ImmutableMultiDict([('user', 'Flask'), ('time', ''), ('p', '7'), ('p', '8')])

```

较新的浏览器也支持直接在url中输入中文（最新的火狐浏览器内部会帮忙将中文转换成符合URL规范的数据），在浏览器中访问`http://127.0.0.1:5000/?info=这是爱，`，将显示：

```xpath
ImmutableMultiDict([('info', '这是爱，')])

```

浏览器传给我们的Flask服务的数据长什么样子呢？可以通过`request.full_path`和`request.path`来看一下：

```xpath
from flask import Flask, request

app = Flask(__name__)


@app.route('/')
def hello_world():
    print(request.path)
    print(request.full_path)
    return request.args.__str__()


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

浏览器访问`http://127.0.0.1:5000/?info=这是爱，`，运行`server.py`的终端会输出：

```shell
/
/?info=%E8%BF%99%E6%98%AF%E7%88%B1%EF%BC%8C

```

### 3.3 获取某个指定的参数

例如，要获取键`info`对应的值，如下修改`server.py`：

```xpath
from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def hello_world():
    return request.args.get('info')

if __name__ == '__main__':
    app.run(port=5000)

```

运行`server.py`，在浏览器中访问`http://127.0.0.1:5000/?info=hello`，浏览器将显示：

```
hello

```

不过，当我们访问`http://127.0.0.1:5000/`时候却出现了500错误

为什么为这样？

这是因为没有在URL参数中找到`info`。所以`request.args.get('info')`返回Python内置的None，而Flask不允许返回None。

解决方法很简单，我们先判断下它是不是None：

```xpath
from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def hello_world():
    r = request.args.get('info')
    if r==None:
        # do something
        return ''
    return r

if __name__ == '__main__':
    app.run(port=5000, debug=True)


```

另外一个方法是，设置默认值，也就是取不到数据时用这个值：

```xpath
from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def hello_world():
    r = request.args.get('info', 'hi')
    return r

if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

函数`request.args.get`的第二个参数用来设置默认值。此时在浏览器访问`http://127.0.0.1:5000/`，将显示：

```
hi

```

### 3.4 如何处理多值

还记得上面有一次请求是这样的吗？

`http://127.0.0.1:5000/?user=Flask&amp;time&amp;p=7&amp;p=8`，仔细看下，`p`有两个值。

如果我们的代码是：

```xpath
from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def hello_world():
    r = request.args.get('p')
    return r

if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

在浏览器中请求时，我们只会看到`7`。如果我们需要把`p`的所有值都获取到，该怎么办？

不用`get`，用`getlist`：

```xpath
from flask import Flask, request

app = Flask(__name__)


@app.route('/')
def hello_world():
    r = request.args.getlist('p')  # 返回一个list
    return str(r)


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

浏览器输入`http://127.0.0.1:5000/?user=Flask&amp;time&amp;p=7&amp;p=8`，我们会看到`['7', '8']`。



## 4. 获取POST方法传送的数据

作为一种HTTP请求方法，POST用于向指定的资源提交要被处理的数据。我们在某网站注册用户、写文章等时候，需要将数据传递到网站服务器中。并不适合将数据放到URL参数中，密码放到URL参数中容易被看到，文章数据又太多，浏览器不一定支持太长长度的URL。这时，一般使用POST方法。

安装方法：

```shell
$ sudo pip3 install requests

```

### 4.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 4.2 查看POST数据内容

以用户注册为例子，我们需要向服务器`/register`传送用户名`name`和密码`password`。如下编写`HelloWorld/server.py`。

```shell
from flask import Flask, request

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/register', methods=['POST'])
def register():
    print(request.headers)
    print(request.stream.read())
    return 'welcome'


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

(‘/register’, methods=[‘POST’])`是指url`/register`只接受POST方法。可以根据需要修改`methods`参数，例如如果想要让它同时支持GET和POST，这样写：

```xpath
@app.route('/register', methods=['GET', 'POST']) 

```

浏览器模拟工具`client.py`内容如下：

```xpath
import requests

user_info = {'name': 'letian', 'password': '123'}
r = requests.post("http://127.0.0.1:5000/register", data=user_info)

print(r.text)

```

运行`HelloWorld/server.py`，然后运行`client.py`。`client.py`将输出：

```
welcome

```

而`HelloWorld/server.py`在终端中输出以下调试信息（通过`print`输出）：

```shell
Host: 127.0.0.1:5000
User-Agent: python-requests/2.19.1
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
Content-Length: 24
Content-Type: application/x-www-form-urlencoded


b'name=letian&amp;password=123'

```

前6行是client.py生成的HTTP请求头，由`print(request.headers)`输出。

请求体的数据，我们通过`print(request.stream.read())`输出，结果是：

```shell
b'name=letian&amp;password=123'

```

### 4.3 解析POST数据

上面，我们看到post的数据内容是：

```shell
b'name=letian&amp;password=123'

```

我们要想办法把我们要的name、password提取出来，怎么做呢？自己写？不用，Flask已经内置了解析器`request.form`。

我们将服务代码改成：

```xpath
from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/register', methods=['POST'])
def register():
    print(request.headers)
    # print(request.stream.read()) # 不要用，否则下面的form取不到数据
    print(request.form)
    print(request.form['name'])
    print(request.form.get('name'))
    print(request.form.getlist('name'))
    print(request.form.get('nickname', default='little apple'))
    return 'welcome'


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

执行`client.py`请求数据，服务器代码会在终端输出：

```shell
Host: 127.0.0.1:5000
User-Agent: python-requests/2.19.1
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
Content-Length: 24
Content-Type: application/x-www-form-urlencoded


ImmutableMultiDict([('name', 'letian'), ('password', '123')])
letian
letian
['letian']
little apple

```

`request.form`会自动解析数据。

`request.form['name']`和`request.form.get('name')`都可以获取`name`对应的值。对于`request.form.get()`可以为参数`default`指定值以作为默认值。所以：

```xpath
print(request.form.get('nickname', default='little apple'))

```

输出的是默认值

```
little apple

```

如果`name`有多个值，可以使用`request.form.getlist('name')`，该方法将返回一个列表。我们将client.py改一下：

```xpath
import requests

user_info = {'name': ['letian', 'letian2'], 'password': '123'}
r = requests.post("http://127.0.0.1:5000/register", data=user_info)

print(r.text)

```

此时运行`client.py`，`print(request.form.getlist('name'))`将输出：

```
[u'letian', u'letian2']

```


## 5. 处理和响应JSON数据

使用 HTTP POST 方法传到网站服务器的数据格式可以有很多种，比如`name=letian&amp;password=123`这种用过`&amp;`符号分割的key-value键值对格式。我们也可以用JSON格式、XML格式。相比XML的重量、规范繁琐，JSON显得非常小巧和易用。

### 5.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 5.2 处理JSON格式的请求数据

如果POST的数据是JSON格式，`request.json`会自动将json数据转换成Python类型（字典或者列表）。

编写`server.py`:

```xpath
from flask import Flask, request

app = Flask("my-app")


@app.route('/')
def hello_world():
    return 'Hello World!'


@app.route('/add', methods=['POST'])
def add():
    print(request.headers)
    print(type(request.json))
    print(request.json)
    result = request.json['a'] + request.json['b']
    return str(result)


if __name__ == '__main__':
    app.run(host='127.0.0.1', port=5000, debug=True)

```

编写`client.py`模拟浏览器请求：

```xpath
import requests

json_data = {'a': 1, 'b': 2}

r = requests.post("http://127.0.0.1:5000/add", json=json_data)

print(r.text)

```

运行`server.py`，然后运行`client.py`，`client.py`会在终端输出：

```
3

```

`server.py`会在终端输出：

```shell
Host: 127.0.0.1:5000
User-Agent: python-requests/2.19.1
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
Content-Length: 16
Content-Type: application/json


&lt;class 'dict'&gt;
{'a': 1, 'b': 2}

```

注意，请求头中`Content-Type`的值是`application/json`。

### 5.3 响应JSON-方案1

响应JSON时，除了要把响应体改成JSON格式，响应头的`Content-Type`也要设置为`application/json`。

编写`server2.py`：

```xpath
from flask import Flask, request, Response
import json

app = Flask("my-app")


@app.route('/')
def hello_world():
    return 'Hello World!'


@app.route('/add', methods=['POST'])
def add():
    result = {'sum': request.json['a'] + request.json['b']}
    return Response(json.dumps(result),  mimetype='application/json')


if __name__ == '__main__':
    app.run(host='127.0.0.1', port=5000, debug=True)

```

修改后运行。

编写`client2.py`：

```xpath
import requests

json_data = {'a': 1, 'b': 2}

r = requests.post("http://127.0.0.1:5000/add", json=json_data)

print(r.headers)
print(r.text)

```

运行`client.py`，将显示：

```
{'Content-Type': 'application/json', 'Content-Length': '10', 'Server': 'Werkzeug/0.14.1 Python/3.6.4', 'Date': 'Sat, 07 Jul 2018 05:23:00 GMT'}
{"sum": 3}

```

上面第一段内容是服务器的响应头，第二段内容是响应体，也就是服务器返回的JSON格式数据。

另外，如果需要服务器的HTTP响应头具有更好的可定制性，比如自定义`Server`，可以如下修改`add()`函数：

```xpath
@app.route('/add', methods=['POST'])
def add():
    result = {'sum': request.json['a'] + request.json['b']}
    resp = Response(json.dumps(result),  mimetype='application/json')
    resp.headers.add('Server', 'python flask')
    return resp

```

`client2.py`运行后会输出：

```
{'Content-Type': 'application/json', 'Content-Length': '10', 'Server': 'python flask', 'Date': 'Sat, 07 Jul 2018 05:26:40 GMT'}
{"sum": 3}

```

### 5.4 响应JSON-方案2

使用 jsonify 工具函数即可。

```xpath
from flask import Flask, request, jsonify

app = Flask("my-app")


@app.route('/')
def hello_world():
    return 'Hello World!'


@app.route('/add', methods=['POST'])
def add():
    result = {'sum': request.json['a'] + request.json['b']}
    return jsonify(result)


if __name__ == '__main__':
    app.run(host='127.0.0.1', port=5000, debug=True)

```




## 6. 上传文件

上传文件，一般也是用POST方法。

### 6.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 6.2 上传文件

这一部分的代码参考自。

我们以上传图片为例： 假设将上传的图片只允许’png’、’jpg’、’jpeg’、’gif’这四种格式，通过url`/upload`使用POST上传，上传的图片存放在服务器端的`static/uploads`目录下。

首先在项目`HelloWorld`中创建目录`static/uploads`：

```shell
mkdir HelloWorld/static/uploads

```

`werkzeug`库可以判断文件名是否安全，例如防止文件名是`../../../a.png`，安装这个库：

```shell
$ sudo pip3 install werkzeug

```

`server.py`代码：

```xpath
from flask import Flask, request

from werkzeug.utils import secure_filename
import os

app = Flask(__name__)

# 文件上传目录
app.config['UPLOAD_FOLDER'] = 'static/uploads/'
# 支持的文件格式
app.config['ALLOWED_EXTENSIONS'] = {'png', 'jpg', 'jpeg', 'gif'}  # 集合类型


# 判断文件名是否是我们支持的格式
def allowed_file(filename):
    return '.' in filename and \
           filename.rsplit('.', 1)[1] in app.config['ALLOWED_EXTENSIONS']


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/upload', methods=['POST'])
def upload():
    upload_file = request.files['image']
    if upload_file and allowed_file(upload_file.filename):
        filename = secure_filename(upload_file.filename)
        # 将文件保存到 static/uploads 目录，文件名同上传时使用的文件名
        upload_file.save(os.path.join(app.root_path, app.config['UPLOAD_FOLDER'], filename))
        return 'info is '+request.form.get('info', '')+'. success'
    else:
        return 'failed'


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

`app.config`中的config是字典的子类，可以用来设置自有的配置信息，也可以设置自己的配置信息。函数`allowed_file(filename)`用来判断`filename`是否有后缀以及后缀是否在`app.config['ALLOWED_EXTENSIONS']`中。

客户端上传的图片必须以`image01`标识。`upload_file`是上传文件对应的对象。`app.root_path`获取`server.py`所在目录在文件系统中的绝对路径。`upload_file.save(path)`用来将`upload_file`保存在服务器的文件系统中，参数最好是绝对路径，否则会报错（网上很多代码都是使用相对路径，但是笔者在使用相对路径时总是报错，说找不到路径）。函数`os.path.join()`用来将使用合适的路径分隔符将路径组合起来。

好了，定制客户端`client.py`：

```xpath
import requests

file_data = {'image': open('Lenna.jpg', 'rb')}

user_info = {'info': 'Lenna'}

r = requests.post("http://127.0.0.1:5000/upload", data=user_info, files=file_data)

print(r.text)

```

运行`client.py`，当前目录下的`Lenna.jpg`将上传到服务器。

然后，我们可以在`static/uploads`中看到文件`Lenna.jpg`。

要控制上产文件的大小，可以设置请求实体的大小，例如：

```xpath
app.config['MAX_CONTENT_LENGTH'] = 16 * 1024 * 1024 #16MB

```

不过，在处理上传文件时候，需要使用`try:...except:...`。

如果要获取上传文件的内容可以：

```xpath
file_content = request.files['image'].stream.read()

```



## 7. Restful URL

简单来说，Restful URL可以看做是对 URL 参数的替代。

### 7.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 7.2 编写代码

编辑server.py：

```xpath
from flask import Flask

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/user/&lt;username&gt;')
def user(username):
    print(username)
    print(type(username))
    return 'hello ' + username


@app.route('/user/&lt;username&gt;/friends')
def user_friends(username):
    print(username)
    print(type(username))
    return 'hello ' + username


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

运行`HelloWorld/server.py`。使用浏览器访问`http://127.0.0.1:5000/user/letian`，HelloWorld/server.py将输出：

```
letian
&lt;class 'str'&gt;

```

而访问`http://127.0.0.1:5000/user/letian/`，响应为404 Not Found。

浏览器访问`http://127.0.0.1:5000/user/letian/friends`，可以看到：

```
Hello letian. They are your friends.

```

`HelloWorld/server.py`输出：

```
letian
&lt;class 'str'&gt;

```

### 7.3 转换类型

由上面的示例可以看出，使用 Restful URL 得到的变量默认为str对象。如果我们需要通过分页显示查询结果，那么需要在url中有数字来指定页数。按照上面方法，可以在获取str类型页数变量后，将其转换为int类型。不过，还有更方便的方法，就是用flask内置的转换机制，即在route中指定该如何转换。

新的服务器代码：

```xpath
from flask import Flask

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/page/&lt;int:num&gt;')
def page(num):
    print(num)
    print(type(num))
    return 'hello world'


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

(‘/page/‘)`会将num变量自动转换成int类型。

运行上面的程序，在浏览器中访问`http://127.0.0.1:5000/page/1`，HelloWorld/server.py将输出如下内容：

```
1
&lt;class 'int'&gt;

```

如果访问的是`http://127.0.0.1:5000/page/asd`，我们会得到404响应。

在官方资料中，说是有3个默认的转换器：

```
int     accepts integers
float     like int but for floating point values
path     like the default but also accepts slashes

```

看起来够用了。

### 7.4 一个有趣的用法

如下编写服务器代码：

```xpath
from flask import Flask

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/page/&lt;int:num1&gt;-&lt;int:num2&gt;')
def page(num1, num2):
    print(num1)
    print(num2)
    return 'hello world'


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

在浏览器中访问`http://127.0.0.1:5000/page/11-22`，`HelloWorld/server.py`会输出：

```
11
22

```

### 7.5 编写转换器

自定义的转换器是一个继承`werkzeug.routing.BaseConverter`的类，修改`to_python`和`to_url`方法即可。`to_python`方法用于将url中的变量转换后供被`包装的函数使用，`to_url`方法用于`flask.url_for`中的参数转换。

下面是一个示例，将`HelloWorld/server.py`修改如下：

```xpath
from flask import Flask, url_for

from werkzeug.routing import BaseConverter


class MyIntConverter(BaseConverter):

    def __init__(self, url_map):
        super(MyIntConverter, self).__init__(url_map)

    def to_python(self, value):
        return int(value)

    def to_url(self, value):
        return value * 2


app = Flask(__name__)
app.url_map.converters['my_int'] = MyIntConverter


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/page/&lt;my_int:num&gt;')
def page(num):
    print(num)
    print(url_for('page', num=123))   # page 对应的是 page函数 ，num 对应对应`/page/&lt;my_int:num&gt;`中的num，必须是str
    return 'hello world'


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

浏览器访问`http://127.0.0.1:5000/page/123`后，`HelloWorld/server.py`的输出信息是：

```
123
/page/123123

```


## 8. 使用`url_for`生成链接

工具函数`url_for`可以让你以软编码的形式生成url，提供开发效率。

### 8.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 8.2 编写代码

编辑`HelloWorld/server.py`：

```xpath
from flask import Flask, url_for

app = Flask(__name__)

@app.route('/')
def hello_world():
    pass

@app.route('/user/&lt;name&gt;')
def user(name):
    pass

@app.route('/page/&lt;int:num&gt;')
def page(num):
    pass

@app.route('/test')
def test():
    print(url_for('hello_world'))
    print(url_for('user', name='letian'))
    print(url_for('page', num=1, q='hadoop mapreduce 10%3'))
    print(url_for('static', filename='uploads/01.jpg'))
    return 'Hello'

if __name__ == '__main__':
    app.run(debug=True)

```

运行`HelloWorld/server.py`。然后在浏览器中访问`http://127.0.0.1:5000/test`，`HelloWorld/server.py`将输出以下信息：

```
/
/user/letian
/page/1?q=hadoop+mapreduce+10%253
/static/uploads/01.jpg

```




## 9. 使用redirect重定向网址

`redirect`函数用于重定向，实现机制很简单，就是向客户端（浏览器）发送一个重定向的HTTP报文，浏览器会去访问报文中指定的url。

### 9.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 9.2 编写代码

使用`redirect`时，给它一个字符串类型的参数就行了。

编辑`HelloWorld/server.py`：

```xpath
from flask import Flask, url_for, redirect

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'hello world'

@app.route('/test1')
def test1():
    print('this is test1')
    return redirect(url_for('test2'))

@app.route('/test2')
def test2():
    print('this is test2')
    return 'this is test2'

if __name__ == '__main__':
    app.run(debug=True)

```

运行`HelloWorld/server.py`，在浏览器中访问`http://127.0.0.1:5000/test1`，浏览器的url会变成`http://127.0.0.1:5000/test2`，并显示：

```
this is test2

```


## 10. 使用Jinja2模板引擎

模板引擎负责MVC中的V（view，视图）这一部分。Flask默认使用Jinja2模板引擎。

Flask与模板相关的函数有：
- flask.render_template(template_name_or_list, **context) Renders a template from the template folder with the given context.

- flask.render_template_string(source, **context) Renders a template from the given template source string with the given context.- flask.get_template_attribute(template_name, attribute) Loads a macro (or variable) a template exports. This can be used to invoke a macro from within Python code.
这其中常用的就是前两个函数。

这个实例中使用了模板继承、if判断、for循环。

### 10.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 10.2 创建并编辑HelloWorld/templates/default.html

内容如下：

```xpath
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;
    
        { if page_title }
            {{ page_title }}
        { endif }
    &lt;/title&gt;
&lt;/head&gt;

&lt;body&gt;
    { block body }{ endblock }


```

可以看到，在``标签中使用了if判断，如果给模板传递了`page_title`变量，显示之，否则，不显示。

标签中定义了一个名为`body`的block，用来被其他模板文件继承。

### 10.3 创建并编辑HelloWorld/templates/user_info.html
内容如下：

```xpath
{ extends "default.html" }

{ block body }
    { for key in user_info }

        {{ key }}: {{ user_info[key] }} 


    { endfor }
{ endblock }

```

变量`user_info`应该是一个字典，for循环用来循环输出键值对。

### 10.4 编辑HelloWorld/server.py

内容如下：

```xpath
from flask import Flask, render_template

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/user')
def user():
    user_info = {
        'name': 'letian',
        'email': '123@aa.com',
        'age':0,
        'github': 'https://github.com/letiantian'
    }
    return render_template('user_info.html', page_title='letian\'s info', user_info=user_info)


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

`render_template()`函数的第一个参数指定模板文件，后面的参数是要传递的数据。

### 10.5 运行与测试

运行HelloWorld/server.py：

```shell
$ python3 HelloWorld/server.py

```

在浏览器中访问`http://127.0.0.1:5000/user`，效果图如下：



查看网页源码：

```xpath
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;
            letian&amp;#39;s info
    &lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
        name: letian &lt;br/&gt;
        email: 123@aa.com &lt;br/&gt;
        age: 0 &lt;br/&gt;
        github: https://github.com/letiantian &lt;br/&gt;
&lt;/body&gt;
&lt;/html&gt;

```



## 11. 自定义404等错误的响应

要处理HTTP错误，可以使用`flask.abort`函数。

### 11.1 示例1：简单入门

建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

代码

编辑`HelloWorld/server.py`：

```xpath
from flask import Flask, render_template_string, abort

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/user')
def user():
    abort(401)  # Unauthorized 未授权
    print('Unauthorized, 请先登录')


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

效果

运行`HelloWorld/server.py`，浏览器访问`http://127.0.0.1:5000/user`，效果如下：



要注意的是，`HelloWorld/server.py`中`abort(401)`后的`print`并没有执行。

### 11.2 示例2：自定义错误页面

代码

将服务器代码改为：

```xpath
from flask import Flask, render_template_string, abort

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/user')
def user():
    abort(401)  # Unauthorized


@app.errorhandler(401)
def page_unauthorized(error):
    return render_template_string('&lt;h1&gt; Unauthorized &lt;/h1&gt;&lt;h2&gt;{<!-- -->{ error_info }}&lt;/h2&gt;', error_info=error), 401


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

`page_unauthorized`函数返回的是一个元组，401 代表HTTP 响应状态码。如果省略401，则响应状态码会变成默认的 200。




## 12. 用户会话

session用来记录用户的登录状态，一般基于cookie实现。

下面是一个简单的示例。

### 12.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 12.2 编辑`HelloWorld/server.py`

内容如下：

```xpath
from flask import Flask, render_template_string, \
    session, request, redirect, url_for

app = Flask(__name__)

app.secret_key = 'F12Zr47j\3yX R~X@H!jLwf/T'


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/login')
def login():
    page = '''
    &lt;form action="{<!-- -->{ url_for('do_login') }}" method="post"&gt;
        &lt;p&gt;name: &lt;input type="text" name="user_name" /&gt;&lt;/p&gt;
        &lt;input type="submit" value="Submit" /&gt;
    &lt;/form&gt;
    '''
    return render_template_string(page)


@app.route('/do_login', methods=['POST'])
def do_login():
    name = request.form.get('user_name')
    session['user_name'] = name
    return 'success'


@app.route('/show')
def show():
    return session['user_name']


@app.route('/logout')
def logout():
    session.pop('user_name', None)
    return redirect(url_for('login'))


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

### 12.3 代码的含义

`app.secret_key`用于给session加密。

在`/login`中将向用户展示一个表单，要求输入一个名字，submit后将数据以post的方式传递给`/do_login`，`/do_login`将名字存放在session中。

如果用户成功登录，访问`/show`时会显示用户的名字。此时，打开firebug等调试工具，选择session面板，会看到有一个cookie的名称为`session`。

`/logout`用于登出，通过将`session`中的`user_name`字段pop即可。Flask中的session基于字典类型实现，调用pop方法时会返回pop的键对应的值；如果要pop的键并不存在，那么返回值是`pop()`的第二个参数。

另外，使用`redirect()`重定向时，一定要在前面加上`return`。

### 12.4 效果

进入`http://127.0.0.1:5000/login`，输入name，点击submit：

进入`http://127.0.0.1:5000/show`查看session中存储的name： 

### 12.5 设置sessin的有效时间

下面这段代码来自：

```xpath
from datetime import timedelta
from flask import session, app

session.permanent = True
app.permanent_session_lifetime = timedelta(minutes=5)

```

这段代码将session的有效时间设置为5分钟。


## 13. 使用Cookie

Cookie是存储在客户端的记录访问者状态的数据。常用的用于记录用户登录状态的session大多是基于cookie实现的。

cookie可以借助`flask.Response`来实现。下面是一个示例。

### 13.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 13.2 代码

修改`HelloWorld/server.py`：

```xpath
from flask import Flask, request, Response, make_response
import time

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/add')
def login():
    res = Response('add cookies')
    res.set_cookie(key='name', value='letian', expires=time.time()+6*60)
    return res


@app.route('/show')
def show():
    return request.cookies.__str__()


@app.route('/del')
def del_cookie():
    res = Response('delete cookies')
    res.set_cookie('name', '', expires=0)
    return res


if __name__ == '__main__':
    app.run(port=5000, debug=True)

```

由上可以看到，可以使用`Response.set_cookie`添加和删除cookie。`expires`参数用来设置cookie有效时间，它的值可以是`datetime`对象或者unix时间戳，笔者使用的是unix时间戳。

```xpath
res.set_cookie(key='name', value='letian', expires=time.time()+6*60)

```

上面的expire参数的值表示cookie在从现在开始的6分钟内都是有效的。

要删除cookie，将expire参数的值设为0即可：

```xpath
res.set_cookie('name', '', expires=0)

```

`set_cookie()`函数的原型如下：

>set_cookie(key, value=’’, max_age=None, expires=None, path=’/‘, domain=None, secure=None, httponly=False) 


>Sets a cookie. The parameters are the same as in the cookie Morsel object in the Python standard library but it accepts unicode data, too. Parameters: 


> key – the key (name) of the cookie to be set. 
 
 

>value – the value of the cookie. 



> max_age – should be a number of seconds, or None (default) if the cookie should last only as long as the client’s browser session. 


> expires – should be a datetime object or UNIX timestamp. 


> domain – if you want to set a cross-domain cookie. For example, domain=”.example.com” will set a cookie that is readable by the domain , foo.example.com etc. Otherwise, a cookie will only be readable by the domain that set it. 


> path – limits the cookie to a given path, per default it will span the whole domain. 


### 13.3 运行与测试

运行`HelloWorld/server.py`：

```shell
$ python3 HelloWorld/server.py

```

使用浏览器打开`http://127.0.0.1:5000/add`，浏览器界面会显示

```
add cookies

```

下面查看一下cookie，如果使用firefox浏览器，可以用firebug插件查看。打开firebug，选择`Cookies`选项，刷新页面，可以看到名为`name`的cookie，其值为`letian`。

在“网络”选项中，可以查看响应头中类似下面内容的设置cookie的HTTP「指令」：

```
Set-Cookie: name=letian; Expires=Sun, 29-Jun-2014 05:16:27 GMT; Path=/

```

在cookie有效期间，使用浏览器访问`http://127.0.0.1:5000/show`，可以看到：

```
{'name': 'letian'}

```

## 14. 闪存系统 flashing system

Flask的闪存系统（flashing system）用于向用户提供反馈信息，这些反馈信息一般是对用户上一次操作的反馈。反馈信息是存储在服务器端的，当服务器向客户端返回反馈信息后，这些反馈信息会被服务器端删除。

下面是一个示例。

### 14.1 建立Flask项目

按照以下命令建立Flask项目HelloWorld:

```shell
mkdir HelloWorld
mkdir HelloWorld/static
mkdir HelloWorld/templates
touch HelloWorld/server.py

```

### 14.2 编写HelloWorld/server.py

内容如下：

```xpath
from flask import Flask, flash, get_flashed_messages
import time

app = Flask(__name__)
app.secret_key = 'some_secret'


@app.route('/')
def index():
    return 'hi'


@app.route('/gen')
def gen():
    info = 'access at '+ time.time().__str__()
    flash(info)
    return info


@app.route('/show1')
def show1():
    return get_flashed_messages().__str__()


@app.route('/show2')
def show2():
    return get_flashed_messages().__str__()


if __name__ == "__main__":
    app.run(port=5000, debug=True)

```

### 14.3 效果

运行服务器：

```shell
$ python3 HelloWorld/server.py

```

打开浏览器，访问`http://127.0.0.1:5000/gen`，浏览器界面显示（注意，时间戳是动态生成的，每次都会不一样，除非并行访问）：

```
access at 1404020982.83

```

查看浏览器的cookie，可以看到`session`，其对应的内容是：

```
.eJyrVopPy0kszkgtVrKKrlZSKIFQSUpWSknhYVXJRm55UYG2tkq1OlDRyHC_rKgIvypPdzcDTxdXA1-XwHLfLEdTfxfPUn8XX6DKWCAEAJKBGq8.BpE6dg.F1VURZa7VqU9bvbC4XIBO9-3Y4Y

```

再一次访问`http://127.0.0.1:5000/gen`，浏览器界面显示：

```
access at 1404021130.32

```

cookie中`session`发生了变化，新的内容是：

```
.eJyrVopPy0kszkgtVrKKrlZSKIFQSUpWSknhYVXJRm55UYG2tkq1OlDRyHC_rKgIvypPdzcDTxdXA1-XwHLfLEdTfxfPUn8XX6DKWLBaMg1yrfCtciz1rfIEGxRbCwAhGjC5.BpE7Cg.Cb_B_k2otqczhknGnpNjQ5u4dqw

```

然后使用浏览器访问`http://127.0.0.1:5000/show1`，浏览器界面显示：

```
['access at 1404020982.83', 'access at 1404021130.32']

```

这个列表中的内容也就是上面的两次访问`http://127.0.0.1:5000/gen`得到的内容。此时，cookie中已经没有`session`了。

如果使用浏览器访问`http://127.0.0.1:5000/show1`或者`http://127.0.0.1:5000/show2`，只会得到：

```
[]

```

### 14.4 高级用法

flash系统也支持对flash的内容进行分类。修改`HelloWorld/server.py`内容：

```xpath
from flask import Flask, flash, get_flashed_messages
import time

app = Flask(__name__)
app.secret_key = 'some_secret'


@app.route('/')
def index():
    return 'hi'


@app.route('/gen')
def gen():
    info = 'access at '+ time.time().__str__()
    flash('show1 '+info, category='show1')
    flash('show2 '+info, category='show2')
    return info


@app.route('/show1')
def show1():
    return get_flashed_messages(category_filter='show1').__str__()

@app.route('/show2')
def show2():
    return get_flashed_messages(category_filter='show2').__str__()


if __name__ == "__main__":
    app.run(port=5000, debug=True)

```

某一时刻，浏览器访问`http://127.0.0.1:5000/gen`，浏览器界面显示：

```
access at 1404022326.39

```

不过，由上面的代码可以知道，此时生成了两个flash信息，但分类(category)不同。

使用浏览器访问`http://127.0.0.1:5000/show1`，得到如下内容：

```
['1 access at 1404022326.39']

```

而继续访问`http://127.0.0.1:5000/show2`，得到的内容为空：

```
[]

```

### 14.5 在模板文件中获取flash的内容

在Flask中，`get_flashed_messages()`默认已经集成到`Jinja2`模板引擎中，易用性很强。下面是来自官方的一个示例：


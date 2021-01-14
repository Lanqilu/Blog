## AJAX

### Ajax是什么

Ajax: asynchronous javascript and xml（异步的javascript和xml）。

Ajax是一种用来改善用户体验的技术，其本质是利用浏览器内置的一种特殊的对象(XMLHttpRequest)异步(即发送请求时，浏览器不会销毁当前页面，用户可以继续在当前页面做其它的操作)的向服务器发送请求,并且利用服务器返回的数据(不再是一个完整的页面，只是部分的数据，一般使用文本或者xml返回)来部分更新当前页面。

使用ajax技术之后，页面无刷新，并且不打断用户的操作。

## Ajax对象

### 如何获得ajax对象

XMLHttpRequest并没有标准化，要区分浏览器:

```javascript
function getXhr()
{
	var xhr;
	if(window.XMLHttpRequest){
		xhr = new XMLHttpRequest();						   // 非ie浏览器
	}else{
		xhr = new ActiveXObject('Microsoft.XMLHttp');   // ie浏览器
	}
}
```

### Ajax对象的属性

+ onreadystatechange: 绑订一个事件处理函数(即: 注册一个监听器) 当ajax对象的readyState值发生了改变(比如， 从0-->1)，就会产生readystatechange事件。
+ responseText: 获得服务器返回的文本
+ responseXML: 获得服务器返回的XML dom对象
+ status:    获得状态码
+ readyState:  返回ajax对象与服务器通讯的状态。返回值是一个number类型的值，不同的值表示不同的含义:
  + 0: (为初始化)  --> 对象已建立，但是尚未初始化(尚未调用 open方法)
  + 1: (初始化)   --> 对象已建立，尚未调用send方法
  + 2: (发送数据)  --> send方法已调用
  + 3: (数据传送中) --> 已接受部分数据
  + 4: (响应结束)  --> 接收了所有的数据

### Ajax编程的基本步骤

1. 获取ajax对象(XmlHttpRequest)
2. 使用 XmlHttpRequest向服务器发送请求
3. 在服务器端处理请求
4. 在监听器当中，处理服务器返回的响应

---

1. 获取ajax对象(XmlHttpRequest)

   ```javascript
   var xhr = getXhr();
   ```

---

2. 发送请求

   ```javascript
   xhr.open(请求方式, 请求地址, 异步还是同步);
   ```

   + 请求方式: get/post
   + 请求地址:如果是get请求，请求参数要添加到请求地址的后面。
   + true表示异步请求: ajax对象发请求的同时，用户可以对当前 页面做其它的操作。**一般常用异步**。
   + false表示同步请求:ajax对象发请求的同时,浏览器会锁订当前页面，用户需要等待处理完成之后才能做下一步操作。

方式一: get请求

```javascript
var xhr = getXhr();
xhr.open('get', 'check_name.action?name=zs', true);
xhr.onreadystatechange=f1;
xhr.send(null);   
```

方式二: post请求

```javascript
var xhr = getXhr();
xhr.open('post', 'check_username.action', true);
// 如果发送的是 post请求，需要设置消息头的编码格式为 “application”
xhr.setRequestHeader('content-type', 'application/x-www-form-urlencoded');
xhr.onreadystatechange=f1;
xhr.send('username=' + $F('username'));
```

注意：因为按照http协议的要求，如果发送的post请求，请求数据包里面，应该有一个消息头 content-type。但是，ajax对象默认没有，所以，需要调用setRequestHeader方法。

---

3. 编写服务器端的代码，服务器端一般不需要返回完整的页面，只需要返回部分的数据，比如一个简单的字符串。

---

4. 编写监听器

```javascript
function f1(){
    if(xhr.readyState == 4){
        //获得服务器返回的数据
        var txt = xhr.responseText;
        //dom操作
    }
}
<script type="text/javascript">
function $(id)			// ajax常用函数的定义
{return document.getElementById(id);}
		
function $F(id)	
{return document.getElementById(id).value;}

function getXhr()		// 获取 XMLHttpRequest
{
    var xhr;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();	// 非ie浏览器
    }else{
        xhr = new ActiveXObject('Microsoft.XMLHttp');   // ie浏览器
    }
}
```

GET方式：

```javascript
function check_name()
{
    // 第一步: 获得 ajax对象
    var xhr = getXhr();
    // 第二步: 发送请求
    xhr.open('get', 'check_name.action?name=' + $F('uname'), true);
    // 第三步: ajax函数: 注册一个事件监听器
    xhr.onreadystatechange = function()	  //此函数为 匿名函数，内部函数
    {
        // 只有ajax对象的readyState值为4时，才能获得服务器返回的数据
        if(xhr.readyState == 4)
        {
            // 获得服务器返回的文本数据
            var txt = xhr.responseText;
			// 更新页面
            ${'name_msg'}.innerHTML = txt;
        }
    }
    $('name_msg').innerHTML = '正在验证....';
    // 第四步: 发送请求
    xhr.send(null);		
}
```

POST方式：

```javascript
function check_name()
{
    // 第一步: 获得 ajax对象
    var xhr = getXhr();
    // 第二步: 发送请求
    xhr.open('post', 'check_name.action', true);
    xhr.setRequestHeader('content-type', 'application/x-www-form-urlencoded')
    // 第三步: ajax函数
    xhr.onreadystatechange = function()	  //此函数为 匿名函数，内部函数
    {
        // 只有ajax对象的readyState值为4时，才能获得服务器返回的数据
        if(xhr.readyState == 4)
        {
            // 获得服务器返回的文本数据
            var txt = xhr.responseText;
            // 更新页面
            ${'name_msg'}.innerHTML = txt;
        }
    }
    $('name_msg').innerHTML = '正在验证....';
    // 第四步: 发送请求
    xhr.send('username=' + $F('uname'));
}
```

用GET 还是 POST？

与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。然而，在以下情况中，请使用 POST 请求：

+ 无法使用缓存文件（更新服务器上的文件或数据库）
+ 向服务器发送大量数据（POST 没有数据量限制）
+ 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

### Ajax编程中的编码问题

发送get请求：

ie浏览器内置的ajax对象，对中文参数使用gbk编码，而其它浏览器(firefox,chrome)使用utf8编码。服务器端默认使用iso-8859-1去解码。

解决方案：

1. 服务器对get请求中的参数使用指定的编码格式进行解码。

   比如: 对于tomcat，可修改conf/server.xml(添加URIEncoding="UTF-8")即: 告诉服务器，对于所有的get请求，使用UTF-8进行编码/解码

2. 对请求地址，使用encodeURI函数进行统一的编码(UTF-8)

   该函数的作用: 对请求地址中的中文进行“UTF-8”编码。

   ```javascript
   var uri = 'check_username.action?username=' + $F{'username'};
   var uri2 = encodeURI(uri);	// 进行编码，欺骗浏览器，防止出现乱码
   xhr.open('get', uri2, true);
   ```

---

Ajax应用中的缓存问题:

当使用IE浏览器时，如果使用get方式发请求，浏览器会将数据缓存起来。

这样，当再此发送请求时，如果请求地址不变，IE浏览器不会真正地向服务器发送请求，而是将之前缓存的数据显示给用户 。

解决方式1: 使用post方式发请求。

解决方式2: 在请求地址后面添加一个随机数:

```javascript
xhr.open('get', 'some?tt=' + Math.random(), true);
```

 
# Content-type

### 常见的媒体格式类型

#####  text/html：html格式

##### text/plaint：纯文本格式

##### image/jpeg：jpg图片格式

##### image/gif ：gif图片格式  

##### image/png：png图片格式

------

### 以application开头的媒体类型

### **application/json**  ： JSON数据格式

### application/xhtml+xml ：XHTML格式

##### application/xml   ： XML数据格式

##### application/atom+xml ：Atom XML聚合格式  

##### application/pdf    ：pdf格式 

##### application/javascript ：js格式

##### application/msword ： Word文档格式

##### application/octet-stream ： 二进制流数据（如常见的文件下载）

##### **application/x-www-form-urlencoded** ：form表单默认的数据格式类型，form表单数据被编码为key/value格式发送到服务器。

------

### 另外一种常见的媒体格式是上传文件之时使用的

##### **multipart/form-data** ： 需要在表单中进行文件上传时，就需要使用该格式。

------

### 常用的媒体格式

+ **application/x-www-form-urlencoded**

提交的数据按照 key1=val1&key2=val2 的方式进行编码，key 和 val 都进行了 URL 转码。大部分服务端语言都对这种方式有很好的支持

```javascript
# Request Headers

POST /adduser HTTP/1.1
Host: localhost:8030
Connection: keep-alive
Content-Length: 16
Pragma: no-cache
Cache-Control: no-cache
Origin: chrome-extension://fdmmgilgnpjigdojojpjoooidkmcomcm
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9

# Form Data

name=name&age=11
```

+ **application/json**

现在越来越多的人把它作为请求头，用来告诉服务端消息主体是序列化后的 JSON 字符串。服务端语言也有很多函数去解析JSON，使用JSON可以支持更加复杂的结构化数据

```js
# Request Headers

POST /adduser HTTP/1.1
Host: localhost:8030
Connection: keep-alive
Content-Length: 24
Pragma: no-cache
Cache-Control: no-cache
Origin: chrome-extension://fdmmgilgnpjigdojojpjoooidkmcomcm
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36
Content-Type: application/json
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9


# Request Payload

{"name":"121","age":121}
```

+ **multipart/form-data**

对用于在表单中上传文件时，也可以上传普通数据，只需要让from的ectyle等于multipart/form-data就可以了

```js
# Request Header

POST /adduser HTTP/1.1
Host: localhost:8030
Connection: keep-alive
Content-Length: 232
Pragma: no-cache
Cache-Control: no-cache
Origin: chrome-extension://fdmmgilgnpjigdojojpjoooidkmcomcm
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryBRi81vNtMyBL97Rb
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9


# Request Payload

------WebKitFormBoundaryBRi81vNtMyBL97Rb
Content-Disposition: form-data; name="name"

name1
------WebKitFormBoundaryBRi81vNtMyBL97Rb
Content-Disposition: form-data; name="age"

12
------WebKitFormBoundaryBRi81vNtMyBL97Rb--
```


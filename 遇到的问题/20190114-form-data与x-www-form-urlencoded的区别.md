# 20190114-form-data与x-www-form-urlencoded的区别 #

## 问题背景描述 ##

使用Postman做接口测试时，发现POST提交数据的方式有多种，如果选择不正确，则没有办法得到正确的结果，这里分析一下各个提交数据的类型有什么区别

## 解决问题方案 ##

**postman中的form-data、x-www-form-urlencoded、raw、binary的区别**

**form-data方式**：表示http请求中的multipart/form-data方式，会将表单的数据处理为一条消息，用分界符隔开，可以上传键值对或者上传文件.比如按照如下方式传输提交数据：

	POST /user/uploadFileAndInfo HTTP/1.1
	Host: 127.0.0.1:8080
	Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
	Cache-Control: no-cache
	Postman-Token: 764425bb-1c2a-668b-b9f1-54afaa15b21b
	
	------WebKitFormBoundary7MA4YWxkTrZu0gW
	Content-Disposition: form-data; name="file"; filename=""
	Content-Type: 
	
	
	------WebKitFormBoundary7MA4YWxkTrZu0gW
	Content-Disposition: form-data; name="user"
	
	{"name": "Lands"}
	------WebKitFormBoundary7MA4YWxkTrZu0gW--

**x-www-form-urlencode方式**：会把表单数据转换为键值对，这种方式传输的是字符类型数据，不能传递文件。

	POST /user/uploadFileAndInfo HTTP/1.1
	Host: 127.0.0.1:8080
	Content-Type: application/x-www-form-urlencoded
	Cache-Control: no-cache
	Postman-Token: 01fedfef-8ab7-fda2-c650-f06505ab9d0c
	
	name=Lands&age=25

**raw方式**： 可以上传任意格式的文本，服务器不会对上传数据做转换处理。选择raw选项后，可以勾选raw文本的格式，格式有

- Text

		POST /user/uploadFileAndInfo HTTP/1.1
		Host: 127.0.0.1:8080
		Cache-Control: no-cache
		Postman-Token: c976ea21-35a3-9421-8b74-19edbc0be42b
		
		{"privateKey": "", "address": "buQWitw9BWRbp2tnTU5R3cjQ3bYthbsKyFEQ", "amount": ""}

- Text(text/plain)

		POST /user/uploadFileAndInfo HTTP/1.1
		Host: 127.0.0.1:8080
		Content-Type: text/plain
		Cache-Control: no-cache
		Postman-Token: eb02a549-74c8-8245-0758-ecf348f3a0f8
		
		{"privateKey": "", "address": "buQWitw9BWRbp2tnTU5R3cjQ3bYthbsKyFEQ", "amount": ""}

- JSON(application/json)

		POST /user/uploadFileAndInfo HTTP/1.1
		Host: 127.0.0.1:8080
		Content-Type: application/json
		Cache-Control: no-cache
		Postman-Token: d84a4b20-cc25-8ca1-7a12-a67451af6ba4
		
		{"privateKey": "", "address": "buQWitw9BWRbp2tnTU5R3cjQ3bYthbsKyFEQ", "amount": ""}

- javascript(application/javascript)

		POST /user/uploadFileAndInfo HTTP/1.1
		Host: 127.0.0.1:8080
		Content-Type: application/javascript
		Cache-Control: no-cache
		Postman-Token: 53a4fcbb-178c-5c66-c272-54009c3eb2f3
		
		{"privateKey": "", "address": "buQWitw9BWRbp2tnTU5R3cjQ3bYthbsKyFEQ", "amount": ""}


- XML(application/xml)

		POST /user/uploadFileAndInfo HTTP/1.1
		Host: 127.0.0.1:8080
		Content-Type: application/xml
		Cache-Control: no-cache
		Postman-Token: 63e37589-b5b5-8204-d26f-714ceb889fd9
		
		{"privateKey": "", "address": "buQWitw9BWRbp2tnTU5R3cjQ3bYthbsKyFEQ", "amount": ""}

- XML(text/xml)

		POST /user/uploadFileAndInfo HTTP/1.1
		Host: 127.0.0.1:8080
		Content-Type: text/xml
		Cache-Control: no-cache
		Postman-Token: d15f78a1-ea1c-9c6a-4808-867064621eee
		
		{"privateKey": "", "address": "buQWitw9BWRbp2tnTU5R3cjQ3bYthbsKyFEQ", "amount": ""}


- HTML(text/html)

		POST /user/uploadFileAndInfo HTTP/1.1
		Host: 127.0.0.1:8080
		Content-Type: text/html
		Cache-Control: no-cache
		Postman-Token: ba4c7942-0427-20ec-d235-4e130a1207d7
		
		{"privateKey": "", "address": "buQWitw9BWRbp2tnTU5R3cjQ3bYthbsKyFEQ", "amount": ""}


**binary方式**：相当于Content-Type:application/octet-stream,只可以上传二进制数据，通常用来上传文件，但是一次只能上传一个文件。

## multipart/form-data与x-www-form-urlencoded的区别 ##
 
multipart/form-data：上传键值对，并且键值对可以包含文件类型，键值对之间通过分界符分割。

x-www-form-urlencoded：上传键值对，键值对只能是字符串类型，键值对都是通过&间隔分开的。
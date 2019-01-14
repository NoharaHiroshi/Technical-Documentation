# 20190114-form-data与x-www-form-urlencoded的区别 #

## 问题背景描述 ##

使用Postman做接口测试时，发现POST提交数据的方式有多种，如果选择不正确，则没有办法得到正确的结果，这里分析一下各个提交数据的类型有什么区别


**postman中的form-data、x-www-form-urlencoded、raw、binary的区别**

form-data方式：表示http请求中的multipart/form-data方式，会将表单的数据处理为一条消息，用分界符隔开，可以上传键值对或者上传文件.比如按照如下方式传输提交数据：

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


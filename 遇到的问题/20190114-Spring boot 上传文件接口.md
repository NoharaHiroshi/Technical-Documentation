20190114-Spring boot 上传文件

## 问题背景描述 ##

通过Spring boot创建接口，有时候会遇到参数类型为File的参数，如何在接口中获取到File对象呢？

## 解决问题方案 ##


通过指定Content-Type: multipart/form-data来传输file对象和其他文本信息

	POST /user/uploadFileAndInfo HTTP/1.1
	Host: 127.0.0.1:8080
	Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
	Cache-Control: no-cache
	Postman-Token: 6441c8a8-eeff-b606-1377-619ade13c49a
	
	------WebKitFormBoundary7MA4YWxkTrZu0gW
	Content-Disposition: form-data; name="file"; filename="123.pdf"
	Content-Type: 
	
	
	------WebKitFormBoundary7MA4YWxkTrZu0gW
	Content-Disposition: form-data; name="user"
	
	{"name": "Lands"}
	------WebKitFormBoundary7MA4YWxkTrZu0gW--

在后台，使用@RequestParam注解通过参数名获取参数，File参数的类型为MultipartFile

	package com.example.demo.controller;

	import org.springframework.boot.autoconfigure.SpringBootApplication;
	// 接收file参数
	import org.springframework.web.multipart.MultipartFile;
	
	@RequestMapping(value = "/uploadFileAndInfo", method = RequestMethod.POST)
	    public String uploadFileAndInfo(
	            @RequestParam(value = "file") MultipartFile file,
	            @RequestParam(value = "user") String user
	    ){
	        System.out.println(file.getContentType());  // application/pdf
	        System.out.println(file.getSize());  // 42462
	        System.out.println(file.getName());  // 123.pdf
	        System.out.println(user); // {"name": "Lands"}
			
	        return "success";
	    }

我们获取到了文件对象，接下来就需要将获取到的文件流保存到服务器上。

**这里我遇到了一个问题，就是使用FileOutputStream创建实例时，提示Unhandled exception: java.io.FileNotFoundException这个错误**
	
	 @RequestMapping(value = "/uploadFileAndInfo", method = RequestMethod.POST)
	    public String uploadFileAndInfo(
	            @RequestParam(value = "file") MultipartFile file,
	            @RequestParam(value = "user") String user
	    ){
	        System.out.println(file.getContentType());
	        System.out.println(file.getSize());
	        System.out.println(file.getName());
	        System.out.println(user);
	        String filePath = "F:" + File.separator + "translation_pdf";
	        System.out.println(filePath);
	        File targetFile = new File(filePath);
	        if(!targetFile.exists()){
	            Boolean isExist = targetFile.mkdirs();
	            System.out.println(isExist);
	        }
	        // Unhandled exception: java.io.FileNotFoundException
	        // 报这个错
	        FileOutputStream out = new FileOutputStream(filePath);
	        out.write(file.getBytes());
	        out.flush();
	        out.close();
	        return "success";
	    }

我检查，路径是存在的，但不知道为什么就是报Unhandled exception: java.io.FileNotFoundException错误，后来发现必须将IO操作放在try...catach语句中才行


	@RequestMapping(value = "/uploadFileAndInfo", method = RequestMethod.POST)
    public String uploadFileAndInfo(
            @RequestParam(value = "file") MultipartFile file,
            @RequestParam(value = "user") String user
    ){
        System.out.println(file.getContentType());
        System.out.println(file.getSize());
        System.out.println(file.getOriginalFilename());
        System.out.println(user);
        String filePath = "F:" + File.separator + "translation_pdf" + File.separator + file.getOriginalFilename();
        File f = new File(filePath);
        if(!f.getParentFile().exists()){ //判断文件父目录是否存在
            f.getParentFile().mkdir();
        }
        try {
            file.transferTo(f);
        } catch (Exception e){
            e.printStackTrace();
        }
        return "success";
    }
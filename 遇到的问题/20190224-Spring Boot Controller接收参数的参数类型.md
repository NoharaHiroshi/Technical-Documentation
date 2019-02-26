# 20190224-Spring Boot Controller接收参数的参数类型 #

请求接口时传递的参数为JSON格式

**1、接收时参数类型为String**
	
	@RequestMapping(value = "/login", method = RequestMethod.POST)
    public ServiceResult login(@RequestBody String params) {
        Gson gson = new Gson();
        JsonObject json = gson.fromJson(params, JsonObject.class);
        String name = json.get("name").toString();
        String password = json.get("password").toString();
        System.out.println(name);
        User user = userService.searchUser(name);
        System.out.println(user);
        return null;
    }

打印params对象

	params = "{"id": "1550996473422628171", name": "Lands", "password": "123456"}"

**2、接收时参数类型为JsonObject**

	@RequestMapping(value = "/login", method = RequestMethod.POST)
    public ServiceResult login(@RequestBody JsonObject params) {
        String name = params.get("name").toString();
        System.out.println(name);
        User user = userService.searchUser(name);
        System.out.println(user);
        return null;
    }

打印params对象

	params = {JsonObject}"{"id":"1550996473422628171","name\"":"Lands","password":"123456"}"
	
展开params对象

	params = {JsonObject}"{"id":"1550996473422628171","name\"":"Lands","password":"123456"}"
	|- members = {LinkedTreeMap} size=3
		|- 0 = {LinkedTreeMap$Node} "id" -> ""1550996473422628171""
		|- 1 = {LinkedTreeMap$Node} "name"" -> ""Lands""
		|- 2 = {LinkedTreeMap$Node} "password" -> ""123456""

接收的参数值发现带有两个"，即""Lands""，所以会报错。


**3、接收时参数类型为Map<String, Object>**

	@RequestMapping(value = "/login", method = RequestMethod.POST)
    public ServiceResult login(@RequestBody Map<String, Object> params) {
        String name = params.get("name").toString();
        System.out.println(name);
        User user = userService.searchUser(name);
        System.out.println(user);
        return null;
    }

打印params对象

	{id=1550996473422628171, name=Lands, password=123456}

展开params对象

	params = {LinkedHashMap}{id=1550996473422628171, name=Lands, password=123456}	
	|- members = {LinkedHashMap} size=3
		|- 0 = {LinkedHashMap$Entry} "id" -> "1550996473422628171"
		|- 1 = {LinkedHashMap$Entry} "name"" -> "Lands"
		|- 2 = {LinkedHashMap$Entry} "password" -> "123456"	

接收的参数值正常，可以使用Map作为接收参数类型。


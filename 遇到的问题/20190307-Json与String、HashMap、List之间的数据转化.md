20190307-String、HashMap、List、Json之间数据的互相转化

## 问题背景描述 ##

在使用Spring Boot时，数据格式的转换一直没有做一个梳理，每次转换的时候还需要去查，因此把转化的规则记录下来，以备后续的使用。

## 解决问题方案 ##

数据类型转换的问题主要存在于从客户端与服务器之间的数据交互，客户端向服务器POST请求多为Json格式，需要按照需要将参数转换成相应的格式，因此以Json做为数据类型交换的中间桥梁。

Json包以阿里的com.alibaba.fastjson为准。

SpringBoot后台接受客户端请求参数时，一般将接收参数的类型设置为String。再将String类型的数据转换成JsonObject类型。

**String => JSONObject**

String类型转换为JSONObject，这是最常用的转换方式。转换后的结果是JSONObject类型。

	String jsonMessage = {"user":"Lands","password":"123456"};
	JSONObject json = JSONObject.fromObject(jsonMessage);

**String => JSONArray**

String转JSONArray时有两种方式，一种是直接通过json.getJSONArray方法获得JSONArray对象。
另一种是通过取出String类型的参数，再通过JSONArray.getJSONArray方法获取JSONArray对象。

	String jsonMessage = {"userList": [{"id": "11", "name": "lll"}, {"id": "12", "name": "222"}]}
	JSONObject json = JSONObject.fromObject(jsonMessage);
	JSONArray jsonArray = json.getJSONArray("selectedIds");

另一种方式

	String jsonMessage = {"userList": [{"id": "11", "name": "lll"}, {"id": "12", "name": "222"}]}
	JSONObject json = JSONObject.parseObject(jsonMessage);
    String userListStr = json.getString("userList");
    JSONArray userList = JSONArray.parseArray(userListStr);

需要额外提一点的是遍历JSONArray不好进行操作，所以要转换成方便操作的类型。

	for(Object user: userList){
		JSONObject item = (JSONObject) user;
	}

**String => JAVA List**

String转JSONArray通常不好操作，更好的解决方式是直接转换成JAVA Object类型。

	String jsonMessage = {"userList": [{"id": "11", "name": "lll"}, {"id": "12", "name": "222"}]}
	JSONObject json = JSONObject.parseObject(jsonMessage);
    String userListStr = json.getString("userList");
    List<User> userList = JSONArray.parseArray(userListStr, User.class);

**String => JAVA Object**

String转JAVA Object类型时，需要使用JSONObject作为中转站。

	String jsonMessage = {"userId":"小明","certificate":{"sealName":"测试印章","sealType":"0","sealImg":""}}
	JSONObject json = JSONObject.parseObject(params);
	String certificateStr = json.get("certificate").toString();
	CustomerCertificate customerCertificate = JSONObject.parseObject(certificateStr, CustomerCertificate.class);
	// 也可以直接转换
	CustomerCertificate customerCertificate = json.parseObject(certificateStr, CustomerCertificate.class);

**String => HashMap**

String转HashMap常用于获取的参数没有所属的对象类型。其实直接转换为JSONObject也可以正常取出值。

	String jsonMessage = {"sealName":"测试印章","sealType":"0","sealImg":""}
	HashMap<String, Object> hashMap = JSONObject.parseObject(
                response_str,
                new TypeToken<Map<String, Object>>(){}.getType());

以上就是String转换其他数据格式的主要方法，基本可以满足需求，有无法实现的再补充。
下面的是处理完数据后，要转换回Json格式返回给客户端。

**JAVA Object => String(JSON)**

	User user = new User();
	String userStr = JSONObject.toJsonString(user);

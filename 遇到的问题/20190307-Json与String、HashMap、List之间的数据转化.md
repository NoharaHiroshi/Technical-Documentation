20190307-String、HashMap、List、Json之间数据的互相转化

## 问题背景描述 ##

在使用Spring Boot时，数据格式的转换一直没有做一个梳理，每次转换的时候还需要去查，因此把转化的规则记录下来，以备后续的使用。

## 解决问题方案 ##

数据类型转换的问题主要存在于从客户端与服务器之间的数据交互，客户端向服务器POST请求多为Json格式，需要按照需要将参数转换成相应的格式，因此以Json做为数据类型交换的中间桥梁。

Json包以阿里的com.alibaba.fastjson为准。

SpringBoot后台接受客户端请求参数时，一般将接收参数的类型设置为String。再将String类型的数据转换成JsonObject类型。

**String => JSONObject**

	String jsonMessage = {"user":"Lands","password":"123456"};
	JSONObject json = JSONObject.fromObject(jsonMessage);

**String => JSONArray**

	String jsonMessage = {"selectedIds":[81,86]};
	JSONObject json = JSONObject.fromObject(jsonMessage);
	JSONArray jsonArray = json.getJSONArray("selectedIds");

**String => JAVA Object**

	String jsonMessage = {"userId":"小明","certificate":{"sealName":"测试印章","sealType":"0","sealImg":""}}
	JSONObject json = JSONObject.parseObject(params);
	String userId = json.getString("userId");
	String certificateStr = json.get("certificate").toString();
	CustomerCertificate customerCertificate = JSONObject.parseObject(certificateStr, CustomerCertificate.class);

**String => HashMap**

	String jsonMessage = {"sealName":"测试印章","sealType":"0","sealImg":""}
	HashMap<String, Object> hashMap = JSONObject.parseObject(
                response_str,
                new TypeToken<Map<String, Object>>(){}.getType());

**JAVA Object => String(JSON)**

	User user = new User();
	String userStr = JSONObject.toJsonString(user);

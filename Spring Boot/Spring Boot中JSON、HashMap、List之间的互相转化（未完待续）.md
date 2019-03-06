# SpringBoot中JSON、HashMap、List之间的互相转化（未完待续） #

当用户通过调用接口，传递过来JSON格式的数据时，SpringBoot需要为接收的参数指定数据类型。

首先需要使用@RequestBody注解来接收请求体中的数据。 然后为参数指定数据类型，可以指定String类型，也可以指定HashMap类型。

**1、以HashMap类型接收参数**

	@RequestMapping(value = "/login", method = RequestMethod.POST)
	public ServiceResult deleteProtocol(@RequestBody Map<String, Object> params) {
	    String selectedIds = params.get("selectedIds").toString();
	    System.out.println(selectedIds);
	    return null;
	}
	
打印params对象

	{selectedIds=[84, 86]}

发现selectedIds的值是List，如何获取这个List的值呢？

（未完待续）
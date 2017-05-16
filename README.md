# ajaxupload

通过jQuery以AJAX方式上传单文件/多文件到服务器端，支持多种服务器端开发语言（包括但不限于Java、NodeJS、PHP、Ruby、.net等）；为便于开发，示例中已兼容了BootStrap3.3.7。

## 环境需求

* jQuery2+，及所需环境（推荐jQuery3）
* 支持AJAX
* 可写的服务器端图片存储文件夹，即图片存储根目录

## WEB前端（以外链JavaScript方式为例）

* 将“ajaxupload.js”文件中“api_url”修改为服务器端处理上传的类方法URL、“uploads_url”修改为图片存储根目录
* 将“ajaxupload.js”文件放入项目的JavaScript资源目录
* 将“client.html”文件中button的data-target-dir属性修改为需要保存待上传图片的子目录名称（下称目标文件夹）
* 将“client.html”文件中示例代码整合入项目对应的视图文件，相应修改外链JavaScript文件的URL

## 服务器端处理流程

1. 创建“status”（int）、“content”（string）、“items”（array/list，视具体服务器端语言而定，下同）类属性；
2. 通过$_GET['target']获取目标文件夹，并根据实际情况决定是否需进行文件夹创建及权限设置；
3. 通过$_FILES获取并依次处理待上传文件;
4. 创建单个文件处理结果数组，含有单个文件上传结果status（int）键值对及单个文件上传详情content（int/string/array/list）键值对；
5. 若文件上传成功，赋值status数组键值为200，否则为400；
6. 若文件上传成功，赋值content数组键值为保存后文件的URL（相对或绝对均可，视具体需要而定），否则赋值content为实际需要的返回信息；
7. 将单个文件处理结果数组推入“items”类属性数组；
8. 若所有文件均上传成功，赋值status类属性为200，否则为400；
9. 若所有文件文件上传失败，赋值content类属性为“上传成功”，否则赋值content为实际需要的返回信息；
10. 通过json格式将所有类属性键值对返回（在不影响上述流程的前提下，可根据实际需要增加其它返回内容）。

## 服务器端响应示例

中文内容已转码以便阅读。

```JavaScript
{
	"status":400,
	"content":"部分上传成功",
	"items":[
		{"status":200,"content":"20170515_110640.png"},
		{"status":200,"content":"20170515_1106401.png"},
		{"status":200,"content":"20170515_1106402.png"},
		{
			"status":400,
			"content":{
				"file":{
					"name":"1227650_all.sql",
					"type":"application\/sql",
					"tmp_name":"\/tmp\/phpOdFKZD",
					"error":0,
					"size":39819
				},
				"descirption":"不可上传该类型的文件"
			}
		}
	],
	"param":{
		"get":{
			"target":"project"
		},
		"post":[]
	},
	"elapsed_time":"0.0246 s"
}
```

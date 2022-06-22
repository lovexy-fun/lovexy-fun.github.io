---
title: js原生导出excel和csv
date: 2019-11-22 14:44:00
tags:
  - JavaScript
---

> 严格意义来说并不是真正的excel文件，只是可以用excel打开查看而已，实际上的格式是逗号分隔文件即csv文件。

这里有几个坑要说一下：

1. 不加Unicode的utf8头部标识excel打开文件会乱码。
2. 加了Unicode的utf8头部标识可能会导致文件读取的时候遇到非法字符。
3. IE不支持a标签的download属性。
4. 这里用的是URL编码，还可以使用base64和blob。

> Unicode头部标识：
>
> EF BB BF	UTF-8 
> FF FE 	UTF-16 aka UCS-2, little endian 
> FE FF 	UTF-16 aka UCS-2, big endian 
> 00 00 FF FE 	UTF-32 aka UCS-4, little endian. 
> 00 00 FE FF 	UTF-32 aka UCS-4, big-endian.

```javascript
/*
data = {
	thead : ["第一列", "第二列", "第三列"],
	tbody : [
		["1", "2", "3"],
		["4", "5", "6"],
	]
}*/
/**
 * 导出excel和csv
 * @param data 要导出的数据，需要是上面的数据格式，当然也可以重写这个方法自己定义数据格式
 * @param name 文件名
 * @param type 文件类型 xls或csv
 * @returns
 */
function exportData(data, name, type) {
	var dataStr = "";
	//Unicode头部标识
	var utf8Head = "%EF%BB%BF";
	//uri文件资源类型
	var csvUri = "data:text/csv;charset=utf-8,";
	var xlsUri = "data:application/vnd.ms-excel;charset=utf-8,";
	//创建一个a标签，用来下载
	var oa = document.createElement("a");
	var col = data.thead.length;
	var row = data.tbody.length;
	
	//数据构造
	for(var i = 0; i < col; i++) {
		dataStr += data.thead[i];
		if(i < col - 1)
			dataStr += ","
	}
	dataStr += "\n";
	
	for(var i = 0; i < row; i++) {
		for(var j = 0; j < col; j++) {
			dataStr += data.tbody[i][j];
			if(j < col - 1)
				dataStr += ",";
		}
		dataStr += "\n";
	}
	if(type === "csv") {
		//拼接编码，用url编码就可以，layui就是这种方式
		oa.href = csvUri + utf8Head + encodeURIComponent(dataStr);
		oa.download = name + ".csv";
	} else if(type === "xls") {
		oa.href = xlsUri + utf8Head + encodeURIComponent(dataStr);
		oa.download = name + ".xls";
	} else {
		return false;
	}
	//触发链接点击事件进行下载
	oa.click();
}
```
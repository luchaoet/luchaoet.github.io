---
title: clipboardData剪切板
date: 2018-11-24 17:57:37
tags: ['js', 'clipboardData']
summary: 复制黏贴功能
---
项目中遇到一个需求，加一个复制的快捷键功能


![20181123182059.png | center | 747x120](https://cdn.nlark.com/yuque/0/2018/png/115449/1542968559046-859b6d23-6c5f-4f0d-8779-2b3adef7d296.png "")


实现原理其实就是点击复制按钮的时候，将需要的内容赋值到剪切板

其实平常浏览网页，比如掘金的时候，也有类似的功能，自定义剪切板内容
用鼠标复制选中的内容


![屏幕快照 2018-11-23 17.23.25.png | center | 651x370](https://cdn.nlark.com/yuque/0/2018/png/115449/1542965126352-88cda2c0-a2ca-497b-a1f1-95be89e1e691.png "")


黏贴至编辑器，发现新增了以下红框中的内容


![20181123172307.png | center | 747x386](https://cdn.nlark.com/yuque/0/2018/png/115449/1542965171760-cdfa9bd7-29b3-479e-bf58-b9277bfbc4e5.png "")


这两个例子其实是一样的，自定义了剪切板的内容

### 语法
```javascript
// 设置剪切板内容
// sDataFormat格式 sData内容
clipboardData.setData(sDataFormat, sData)
```

### 示例
```javascript
// 全局监听copy事件
document.addEventListener('copy', function (event) {
    var clipboardData = event.clipboardData || window.clipboardData;
    if (clipboardData) { 
        event.preventDefault();
        // 获取剪切板的内容
        const text = window.getSelection().toString();
        // 修改剪切板的内容
        clipboardData.setData(
            'text',
            text +
            '\n作者：长剑腥腥挂壁'+
            '\n链接：https://lucy20209060.github.io/2018/11/24/clipboardData剪切板/'+
            '\n来源：长剑腥腥挂壁博客'
        )
        console.log('复制成功')
    }else{
        console.log('复制失败')
    }
});
```

复制黏贴后
```plain
	哈哈哈哈
	作者：长剑腥腥挂壁
	链接：https://lucy20209060.github.io/2018/11/24/clipboardData剪切板/
	来源：长剑腥腥挂壁博客
```

正是我们预期的效果

这是文本，现在又有一个问题，假如复制一张图片呢，比如我编辑语雀文档的时候，就可以直接在文档中黏贴图片

我们做个例子


![屏幕快照 2018-11-24 12.16.19.png | center | 747x416](https://cdn.nlark.com/yuque/0/2018/png/115449/1543033003587-07f15244-1926-4845-ba77-8f07c7ee28e3.png "")


在页面随意复制下面的某张图片，黏贴的时候把图片放在预览区域

```javascript
// 黏贴事件监听
document.addEventListener('paste', function (event) {
	var items = event.clipboardData && event.clipboardData.items;
	var file = null;
	if (items && items.length) {
		// 循环操作 此时会将最后一张图片文件赋值给 file
		for (var i = 0; i < items.length; i++) {
			if (items[i].type.indexOf('image') !== -1) {
				file = items[i].getAsFile();
				break;
			}
		}
	}
    // 此处可以进行图片上传的操作
    // 下面是获取图片进行预览
    var reader = new FileReader()
	reader.onload = function(event) {
		// event.target.result 
       // Base64格式图片
		document.getElementsByClassName('view-image')[0].innerHTML = '<img height="100px" src="'+ event.target.result +'" />'
	}
	reader.readAsDataURL(file);
});
```

效果
![屏幕快照 2018-11-24 12.27.39.png | center | 220x304.45820433436535](https://cdn.nlark.com/yuque/0/2018/png/115449/1543033796827-87661112-5653-473c-8946-a28e9e54289e.png "")![屏幕快照 2018-11-24 12.27.55.png | center | 220x317.20930232558135](https://cdn.nlark.com/yuque/0/2018/png/115449/1543033712804-41c4cdaf-c2e0-4b31-a999-5b154bb7b180.png "")
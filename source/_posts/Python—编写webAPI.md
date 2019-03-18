---
title: Python—编写web API
date: 2019-02-20 21:26:59
tags: ['Python', 'API']
summary:
---
### Flask
Flask是一个使用 Python 编写的轻量级 Web 应用框架
```python
from flask import Flask, request, render_template, jsonify
# 从fetch.py文件中导入menu_list方法，该方法是从数据库获取菜单列表
# 关于python如何操作数据库，前面我有篇文章单独讲过，在此不述
from fetch import menu_list

app = Flask(__name__)

# home.html页面的路由
@app.route('/', methods=['GET', 'POST'])
def home():
    return render_template('home.html',title='页面')

# 编写的web api
@app.route('/rpa/menu', methods=['GET'])
def list():
    res = menu_list()
    # 返回序列化数据
    return jsonify(res)

if __name__ == '__main__':
    app.run(debug=True, port='8080', host='127.0.0.1')
```

### home.html
在页面中通过ajax获取api接口请求，python API获取数据库数据并返回
```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
</head>
<body>
    <h1>Home Page{{ title }}</h1>
    <ul></ul>
    <button onclick="buttonClick()">点击</button>
</body>
    <script>
        function buttonClick() {
            $.ajax({
                url: '/rpa/menu',
                type: 'get',
                dataType: 'json',
                data: {},
                success:(res)=>{
                    console.log(res)
                    for(let item of res){
                        $('ul').append('<li>'+ item.name +'</li>')
                    }
                }
            })
        }
    </script>
</html>
```
### 
### 返回的结果
![屏幕快照 2019-02-19 11.41.52.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1550547746261-24a32246-1e59-4668-bf54-88f371ccad1e.png#align=left&display=inline&height=332&linkTarget=_blank&name=%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-02-19%2011.41.52.png&originHeight=332&originWidth=452&size=39692&width=452)<br />我们便用python打通了前端，后端和数据库这一条链路<br />这就是简单的web API编写过程
---
title: nginx部署前端单页面项目
date: 2019-04-16 00:53:37
tags: ['nginx', '单页面']
summary:
---
vue/react等build打包完成后，需将打包后的静态资源拿去部署<br />当nginx部署这些单页面时会遇到一个问题，几年前还因为这个问题跟后端小哥撕过逼，我一直说是nginx配置的问题，固执小哥一直说是前端代码的问题，当年不懂nginx，也说不出个所以然

从首页跳转至`/about`页面是正常的<br />![20190416002651.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1555346567838-909eb8a1-40d8-4fe0-9d80-42cd12eb73c9.jpeg#align=left&display=inline&height=216&name=20190416002651.jpg&originHeight=434&originWidth=1500&size=47272&status=done&width=746)<br />一旦点击浏览器的刷新按钮刷新页面，就出现了404的问题![20190416002728.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1555346617764-57906c7c-d70b-4a8c-824a-6cfc4d8e4c8f.jpeg#align=left&display=inline&height=223&name=20190416002728.jpg&originHeight=448&originWidth=1496&size=57354&status=done&width=746)其实只需在`location`中追加一行`try_files $uri $uri/ /index.html;`即可
```nginx
location / {
  root    /var/www/dist;
  index   index.html;
  try_files $uri $uri/ /index.html;
}
```
在访问SPA单页面时，实际上访问的是单页面的路由，例如`/about`，对应的其实是单页面路由中配置的路由组件。但是对于nginx服务器来讲，被认为会寻找根目录下about文件夹，显示nginx目录下没有该文件。单页面实际上只有一个index.html页面，因此将所有的页面都rewirte到index.html页面，即可完成配置，把路由交给前端去处理
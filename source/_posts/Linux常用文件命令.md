---
title: Linux常用文件命令
date: 2019-04-11 18:38:49
tags: Linux
summary:
---
`rm -rf /test` 删除test文件夹及其下所有文件<br />`rm -f /test/index.html` 删除index.html文件<br />-r 就是向下递归，不管有多少级目录，一并删除<br />-f 就是直接强行删除，不作任何提示的意思

cd 进入目录<br />pwd 显示当前路径<br />ls 显示当前目录下的文件名及文件夹名<br />ls -l/ll 显示文件和目录的详细资料 <br /><br /><br />mkdir test 创建test文件夹 可同时创建多个文件夹<br />mkdir -p /tmp/dir1/dir2 创建一个目录树<br /><br />find /test -name file1 寻找/test目录下name为file1的文件<br /><br /><br />cat /test/index.html 从头开始查看文件<br /><br /><br />cp /Users/index.html ./ 复制/Users目录下的index.html至当前目录 `./`表示当前目录<br />cp -r /Users/build ./ 复制/Users目录下的build文件夹至当前目录

touch index.html 创建文件

vi /test/index.html 编辑文件
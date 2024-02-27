---
layout: post
title: MySQL备份还原
category: tutorial
thumbnail: /style/image/thumbnail.png
icon: book
---


* content
  {:toc}




```dos
PS C:\Users\月与海丶> mysqldump -u root -p -B wxh_dp01 > d:\\bak.sql
Enter password: ***
```

Mysql 在dos管理员环境下备份语句如上，-B后为需要备份的数据库，且可一次性添加多个，被分为>后面的文件。

这个备份实质就是恢复成sql语句

------

现在测试，备份后，删除数据库，进入mysql环境后，使用source d:\\bak.sql 但是需要注意可能需要通过代码改变编码

```
mysqldump -u 用户名 -p 数据库 表1 表2 表n >d:\\文件名.sql
这种可以直接备份数据库的表而不用直接备份整个数据库
```


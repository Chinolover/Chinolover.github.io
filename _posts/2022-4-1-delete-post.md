---
layout: post
title: Delete
category: Mysql
thumbnail: /style/image/thumbnail.png
icon: book
---


* content
  {:toc}


```mysql
#delete 演示

SELECT * FROM goods
#1.删除华为手机记录
DELETE FROM goods
				WHERE goods_name = '华为手机'
				
#2.删除所有记录（慎用）
DELETE FROM goods
```


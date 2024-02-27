---
layout: post
title: Update
category: Mysql
thumbnail: /style/image/thumbnail.png
icon: book
---


* content
  {:toc}



```mysql
#演示update

SELECT * FROM goods
#1.将所有手机价格改为5000 （慎用）
UPDATE goods SET price = 5000

#2.将vivo价格改为3000
UPDATE goods 
				SET price = 3000
				WHERE goods_name = 'vivo'
				
#3.将iphone手机价格原基础上增加1000
UPDATE goods
				SET price = price + 1000
				WHERE goods_name = 'iphone'
				
#如果要修改多个字段，可以通过 set 字段1=值1,字段2=值2....
```


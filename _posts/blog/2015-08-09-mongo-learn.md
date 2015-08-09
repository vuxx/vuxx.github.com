---
layout: post
title: mongo 学习笔记(1)
description: 基本知识mongo
category: blog
---
##基本命令
		
		use databasename	选库
		show tables/collections 查看当前库下的collections
		db.collection.drop()
		

###CURD
####C
		db.collection.insert();

####D
		###查询表达式josn对象,ture删一行
		db.collection.remove(查询表达式,option);
####U	
		db.collection.update(查询表达式,新值,option); 
		db.collection.update(query,{$set:{name:'xx'}},opiton);
		$set 指定修改某列的值
		$unset -->删除列
		$rename 从命名列明
		$inc 增长某列的值
		$setOnInsert 当upsert为true,并且inert发生，可以补充的字段
		---------option--
		{upsert:true/false,multi:true/false}
		upsert:没有指定的列，插入
		multi:更新多行

		例db.stu.update({sn:5},{$set:{sn:5},$setOnInsert:{name:'haha'}},{upsert:1});
####R

		db.collection.find(query,{列:1,lie:0});
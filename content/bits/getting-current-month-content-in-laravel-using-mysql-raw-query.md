+++
type="post"
toc=false
title= "Getting Current Month Content in Laravel Using Mysql Raw Query"
date= 2018-11-12T01:24:29+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
bit_tags= ['laravel', 'mysql']
tutorialTypes=['bits']
keyword= "laravel, mysql query"
description= "How to get current month result in laravel mysql select query"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

current months result
~~~php
$this_month = date('n');
$ratings_last_month = DB::select("select count(id) as c, rating from ratings where (user_id=$id and MONTH(created_at) = $this_month  ) group by `rating` order by rating");
~~~

table description

~~~bash
+------------+------------------+------+-----+---------+----------------+
| Field      | Type             | Null | Key | Default | Extra          |
+------------+------------------+------+-----+---------+----------------+
| id         | int(10) unsigned | NO   | PRI | NULL    | auto_increment |
| rating     | int(11)          | NO   |     | NULL    |                |
| ip_address | varchar(255)     | NO   |     | NULL    |                |
| user_id    | int(11)          | NO   |     | NULL    |                |
| created_at | timestamp        | YES  |     | NULL    |                |
| updated_at | timestamp        | YES  |     | NULL    |                |
+------------+------------------+------+-----+---------+----------------+
~~~

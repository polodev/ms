+++
type="post"
toc=false
title= "Export and Import Json in Mongodb"
date= 2018-11-12T01:24:29+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
bit_tags= ['mongodb', 'export', 'import']
tutorialTypes=['bits']
keyword= "mongodb, export, import"
description= "Export and Import Json in Mongodb"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++



# exporting in mongo db

~~~bash
mongoexport -d database_name -c collection_name -o file_name

~~~

# importing in mongodb

~~~bash
mongoimport -d datbase_name -c collection_name file_name
~~~

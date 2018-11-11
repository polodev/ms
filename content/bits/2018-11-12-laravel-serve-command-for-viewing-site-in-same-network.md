+++
type="post"
toc=false
title= "Laravel Serve Command for Viewing Site in Same Network"
date= 2018-11-12T01:24:29+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
bit_tags= ['laravel', 'artisan command']
tutorialTypes=['bits']
keyword= "laravel, artisan command"
description= "Laravel Serve Command for Viewing Site in Same Network"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

# to find ip address // inet will be host
~~~bash
ifconfig | grep 192.168
~~~
# to serve
~~~bash
sudo php artisan serve --host=192.168.31.254 --port=80
~~~


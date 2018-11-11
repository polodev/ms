+++
type="post"
toc=true
title= "Installing Robo3t in Ubuntu 18.X"
date= 2018-11-12T01:23:52+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
tutorial_tags= ['ubuntu', 'robo3t', 'mongodb']
tutorialTypes=['tutorials']
keyword= "ubuntu, robo3t, mongodb"
description= "How to install robo3t in ubuntu 18.X"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

download robomongo 3t from their website. Let downloaded file name is `robomongo-0.9.0-rc8-linux-x86_64-c113244.tar.gz`


extract `robomongo-0.9.0-rc8-linux-x86_64-c113244.tar.gz`  file



move extracted folder to `/usr/bin` directory by following command
~~~bash
sudo mv robomongo-0.9.0-rc8-linux-x86_64-c113244/ /usr/bin/robomongo
~~~

add `export PATH=/usr/bin/robomongo/bin:$PATH` in your .bashrc or .zshrc

If you are using fish shell, you need to change the last line to:

`set PATH $PATH /usr/bin/robomongo/bin`


now start robo3t by typing following in command prompt
~~~bash
robo3t
~~~

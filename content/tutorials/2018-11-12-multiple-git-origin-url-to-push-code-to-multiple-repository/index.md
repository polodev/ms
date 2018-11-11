+++
type="post"
toc=false
title= "Multiple git origin remote url to push code to multiple git repository"
date= 2018-11-12T01:23:53+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
tutorial_tags= ['git', 'git remote url', 'multiple repo']
tutorialTypes=['tutorials']
keyword= "git, git remote url, multiple repo, git command, git delete remote origin, add remote origin"
description= "Here I will find how can you add multiple git remote url in you git. You can send your code to multiple repository in same time. you can delete origin and update origin as well"
skill_level=["beginner"]
+++

I have stored a project in my bitbucket. At the same time I wanted to keep these code in my github account so by adding multiple git remote origin url I am able to push code to different code repo.

following command for adding remote

~~~bash
git remote set-url --add --push origin https://github.com/username/projectname.git
git remote set-url --add --push origin https://username@bitbucket.org/username/project.git
~~~

If you need to delete origin
~~~bash
git remote remove origin
~~~

Once you have delete origin you might need to add remote url again

~~~bash
git remote add origin https://github.com/username/projectname.git
~~~

if you want to change your remote origin in some point

~~~bash
git remote set-url origin https://github.com/username/projectname.git
~~~



+++
type="post"
toc=false
title= "Flatten Array in Php"
date= 2018-11-12T01:23:52+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
bit_tags= ['php', 'functions', 'flatten array']
tutorialTypes=['bits']
keyword= "flatten array"
description= "flatten array in php"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++


### following code for `int` type in my case. just change function appropriately when you want to change


~~~php
public function flattenUnique(array $array) {
  $return = array();
  array_walk_recursive($array, function($a) use (&$return) { $return[] = intval($a); });
  return array_unique( $return );
}
~~~





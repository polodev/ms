+++
type="post"
toc=true
title= "Next and Previous Element in Php Array"
date= 2018-11-12T01:23:53+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
language=['php']
tutorial_tags= ['php functions', 'php array']
tutorialTypes=['tutorials']
keyword= "php functions, next element in php array, previous elements in php array"
description= "how to find next and previous element from php array"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

# getting next element from array

~~~php
function get_next_element ($needle, $haystack) {
  $index = array_search($needle, $haystack);
  if (count($haystack) < 2) {
    return false;
  }
  if ($index < (count($haystack) - 1)) {
    $index++;
  }else {
    $index = 0;
  }
  return $haystack[$index];
}
~~~

## Previous Element
~~~php
function get_prev_elemnt($needle, $haystack) {

  if (count($haystack) < 2) {
    return false;
  }
  $index = array_search($needle, $haystack);
  if ($index == 0 || $index < 1) {
    $index = count($haystack) - 1;
  }else {
    $index--;
  }
  return $haystack[$index];
}

~~~

## Testing

~~~php
$needle = 3;
$haystack = [3, 2, 4, 5, 6];
echo get_prev_elemnt($needle, $haystack);
~~~


+++
type="post"
toc=true
title= "Php Functions - array_values, json_encode, json_decode, array_intersect"
date= 2018-11-12T01:23:53+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
language=['php']
tutorial_tags= ['php functions', 'php']
tutorialTypes=['tutorials']
keyword= "php functions - array_values, json_encode, json_decode, array_intersect"
description= "Basic manipulation using php function - array_values, json_encode, json_decode, array_intersect"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++


~~~php
array_values()
json_encode();
json_decode()
array_intersect()
~~~

## `array_values()`

It will give removed key of associated array

~~~php
$a = [
  'f1' => 'banana',
  'f2' => 'apple',
  'f3' => 'orange',
];
print_r (array_values($a));
~~~

Output for state above code
~~~json
Array
(
    [0] => banana
    [1] => apple
    [2] => orange
)
~~~

## `json_encode()` - encode php to json
~~~php
$a = [
  'f1' => 'banana',
  'f2' => 'apple',
  'f3' => 'orange',
];

$b = json_encode($a);

print_r( $b );
~~~

output
~~~js
{
  "f1": "banana",
  "f2": "apple",
  "f3": "orange"
}
~~~


## `json_decode()` - decode json to php
After encode to json if we require to decode or when we store value as array in mysql or any other database we need to decode to php again. In this case we use `json_decode()`
~~~php
$b = json_encode($a);
$c = json_decode($b);
print_r( $c );
~~~

output of json_decode
~~~js
stdClass Object
(
    [f1] => banana
    [f2] => apple
    [f3] => orange
)
~~~

## `array_intersect()` showing common elements from 2 or more array (in Bengali language it means ga, sa, gu)

~~~php
$a = ['a', 'b', 'c', 'd', 'f', 'g'];
$b = ['a', 'b', 'd', 'g', 'h'];
$c = array_intersect($a, $b);
print_r($c);
~~~

output of the state above code

~~~php
Array
(
    [0] => a
    [1] => b
    [3] => d
    [5] => g
)
~~~














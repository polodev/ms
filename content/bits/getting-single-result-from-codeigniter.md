+++
type="post"
toc=false
title= "Getting Single Result From Codeigniter"
date= 2018-11-12T01:24:29+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
bit_tags= ['codeigniter', 'database query']
tutorialTypes=['bits']
keyword= "codeigniter, database query"
description= "How to run database query for getting single result in codeigniter"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++


~~~php

public function single_result($id)
{
    $this->db->select( '*' );
    $this->db->where( 'id',  $id);
    $q = $this->db->get( 'attachments' );
    $first = $q->row();
    return $first;
}
~~~

+++
type="post"
toc=false
title= "Insert Into Database in Codeigniter"
date= 2018-11-12T01:24:29+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
bit_tags= ['codeigniter', 'database insert query', 'sql']
tutorialTypes=['bits']
keyword= "codeigniter, database insert query, sql"
description= "Insert Into Database in Codeigniter"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++


~~~php

public function insert_attachment($name, $path, $clients_web_id)
{
    $data =[
      'name' => $name,
      'path' => $path,
      'clients_web_id' => $clients_web_id
    ];

    $this->db->insert('attachments', $data);
    $id = $this->db->insert_id();        // return $this->db->
    $this->db->select( '*' );
    $this->db->where( 'id',  $id);
    $q = $this->db->get( 'attachments' );
    $first = $q->row();
    return $first;
    // return $this->attachments_by_clients_web_id($clients_web_id);
}
~~~


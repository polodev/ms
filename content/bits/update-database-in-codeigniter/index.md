+++
type="post"
toc=true
title= "Update Database in Codeigniter"
date= 2018-11-12T01:23:52+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
bit_tags= ['codeigniter', 'php']
tutorialTypes=['bits']
keyword= "update database in codeigniter"
description= "update database in codeigniter"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

~~~php
 public function upload_doc() {
    $id = $this->session->userdata( 'id' );
    $data = array(
      'heading' => $this->input->post('heading'),
    );
    $this->db->where( 'id', $id );
    $this->db->update( 'clients_web', $data );
    $this->session->set_flashdata( 'success', 'All updated successfully' );
    redirect( '/account/upload' );
}
~~~

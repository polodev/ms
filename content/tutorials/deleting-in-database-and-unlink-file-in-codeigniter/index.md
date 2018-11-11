+++
type="post"
toc=true
title= "Deleting in Database and Unlink File in Codeigniter"
date= 2018-11-12T01:23:53+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
language=['php']
tutorial_tags= ['codeigniter', 'database', 'file delete', 'unlink']
tutorialTypes=['tutorial']
keyword= "codeigniter, database, file delete, unlink"
description= "How delete database record in codeigniter and delete associate file"
skill_level=["intermediate"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

# controller code
~~~php
public function delete_attachment() {
  $id = $this->input->post( 'id') ;
  $this->db->select( '*' );
  $this->db->where( 'id',  $id);
  $q = $this->db->get( 'attachments' );
  $first = $q->row();
  $this->db->delete('attachments', array('id' => $id));
  if($this->db->affected_rows() >= 1){
      $upload_path = './assets/user/images/' . $first->path;
      if(unlink($upload_path)) {
        $this->attachments_by_clients_web_id();
      }
  } else {
      $this->attachments_by_clients_web_id();
  }
  return;
}
public function attachments_by_clients_web_id()
{
  $id = $this->session->userdata( 'id' );
  $attachments = $this->user_model->attachments_by_clients_web_id($id);
  echo json_encode($attachments);
}
~~~


# model

~~~php
// return whole results after deleting
public function attachments_by_clients_web_id($clients_web_id)
{
   $where['clients_web_id'] = $clients_web_id;
   $result_set = $this->db->get_where('attachments', $where);
   return $result_set->result_array();
}
~~~






+++
type="post"
toc=true
title= "Setting and Getting User Data in Session Session Flash Codeigniter"
date= 2018-11-12T01:23:53+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
language=['php']
tutorial_tags= ['codeigniter', 'session', 'session flash']
tutorialTypes=['tutorials']
keyword= "codeigniter, session"
description= "Setting and Getting User Data in Session Session Flash Codeigniter"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

# setting user data
~~~php
 $session_data = array(
   'type' => "admin",
   'email'     =>     $email,
   'id'   => $res->id,
   'username'   => $res->username,
   'password'   => $res->password,
);
$this->session->set_userdata($session_data);
~~~

# getting user data
~~~php
$id = $this->session->userdata( 'id' );
~~~


# setting flash message in session

~~~php
$this->session->set_flashdata( 'success', 'All updated successfully' );
~~~

# getting flash message from session

~~~html
<?php if ($this->session->flashdata('success')) { ?>
  <div class="alert alert-success"> <?= $this->session->flashdata('success') ?> </div>
<?php } ?>

<?php if ($this->session->flashdata('error')) { ?>
  <div class="alert alert-danger"> <?= $this->session->flashdata('error') ?> </div>
<?php } ?>
~~~



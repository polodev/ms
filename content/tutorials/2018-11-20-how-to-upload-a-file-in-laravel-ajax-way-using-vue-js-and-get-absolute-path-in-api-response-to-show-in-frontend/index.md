+++
type="post"
toc=true
title= "How to Upload a File in Laravel Ajax Way Using Vue Js and Get Absolute Path in Api Response to Show in Frontend"
date= 2018-11-20T19:47:55+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
language=['php']
software=['']
tutorial_tags= ['laravel', 'file upload', 'ajax', 'vue', 'axios',]
tutorialTypes=['tutorials']
keyword= "laravel, file upload, ajax file upload, vue file upload, laravel file upload using vue, axios file upload"
description= "I have shown how to upload file in laravel using vue component and axios. and how to get absolute path when dealing with api call in laravel "
skill_level=["intermediate"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++


<div class='text-center my-4'>
  <img src='laravel_vue_file_upload.png' style="max-width: 60%" alt='laravel_vue_file_upload'/>
</div>

# function for uploading video
{{< fd "VideoController.php" >}}
~~~php
public function post_video_details(Request $request)
{
    $uploadedFile = $request->file('file');

    $without_extension = pathinfo($uploadedFile->getClientOriginalName(), PATHINFO_FILENAME); // getting only file name without extension to make it slugify
    $extension = $uploadedFile->getClientOriginalExtension();
    $without_extension =  str_slug($without_extension); // make slugify only name
    $filename = time().$without_extension.'.'.$extension; // time() + name + . + extension
    Storage::disk('local')->putFileAs(
      'public/uploads/',
      $uploadedFile,
      $filename
    );

    // putFileAs take 3 parameters. 1.folder, 2.file 3.my_desired_filename


    Video::create([
      'title' => $request->title,
      'video' => $filename,
      'user_id' => auth()->id(),
    ]); // adding info to the database

    return response()->json([
      'success' => 'Video Details update successfully',
    ]);

}
~~~

# transforming single instance in model for api response


{{< fd "Video.php" >}}
~~~php
<?php

namespace App;

use App\User;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Support\Facades\Storage;

class Video extends Model
{
  protected $guarded = [];

  protected $appends = ['video_url'];
  public function getVideoUrlAttribute() {
    return url( Storage::url('uploads/'.$this->video));
  }

  public function user()
  {
    return $this->belongsTo(User::class);
  }
}

~~~

Here we append `video_url` as a properties. Properties value will be resolved by `getVideoUrlAttribute`.


# vue component for showing and uploading content

{{< fd "Video.vue html part" >}}

~~~html
<template>
  <transition name="fade">
    <div class='video_block'>
      <div class='card'>
        <div class='card-header'>
          Videos
        </div>
        <div class='card-body'>

          <h3 class="my-4"> <span class='text-dark'>Introduction Video. </span> Introduce yourself and tell others about what you are looking for?</h3>

          <div class="form-group image-div">
              <input
                id="image_upload"
                name="image"
                v-on:change="handleFileChange()"
                accept="video/mp4,video/x-m4v,video/*"
                ref="file"
                placeholder=""
                type="file">
              <label id="image_upload_label" for="image_upload"><i class="fa fa-file-video-o" aria-hidden="true"></i> Upload a Video</label>
          </div>
          <div>
              <label v-if="formErrors.file" class='d-block text-danger'>Video is required</label>
          </div>
          <div v-if="file" class='form-group'>
            <p>{{file.name}} <span @click="removeFile" class="span_times"><i class='fa fa-times'></i></span></p>
          </div>
          <div class='form-group mt-4'>
            <label for='video_title'>Video title</label>
            <input v-model="title" type='text' class='form-control' name='video_title' id='video_title'/>
            <label v-if="formErrors.title" class='d-block text-danger'>title is required</label>
          </div>

            <div class='row'>
              <div class='col-md-12'>
                <div class="d-flex justify-content-end mt-3">
                    <button v-if="!loading" class="btn btn-warning mr-3" @click="resetForm">
                      Reset
                    </button>
                    <button v-if="!loading" class="btn btn-primary" @click="handleSubmit">
                      Save
                    </button>
                    <button v-if="loading" class='btn btn-primary'>
                      <i class="fa fa-circle-o-notch fa-spin" style="font-size:24px"></i>
                      {{uploadPercentage.toFixed(2)}}%
                    </button>
                </div>
              </div>
            </div>

        </div>
      </div>

      <div class='card mt-4'>
        <div class='card-header'>
          <h2>My videos</h2>
        </div>
        <!-- /.card-header -->
        <div class='card-body'>
          <div v-if="videos.length" class='row'>

            <div v-for="item in videos" class='col-md-6'>
              <div class='card m-2'>
                <div class='card-body mx-auto'>
                  <h5>{{item.title}}</h5>
                  <div>
                    <video width='100%' controls :src='item.video_url'></video>
                  </div>
                  <div class="d-flex justify-content-center mt-3">
                    <button @click="delete_video_item(item.id)" class="btn btn-danger">delete</button>
                  </div>
                </div>
              </div>
            </div>




          </div>
          <p v-if="!videos.length">You haven't upload any video yet</p>
          <!-- /.row -->
        </div>
      </div>
      <!-- /.card -->

    </div>
  </transition>
</template>
~~~

{{< fd "Video.vue js part" >}}

~~~js
export default {
  data() {
    return {
      file: '',
      title: '',
      loading: false,
      formErrors: {},
      success: false,
      error: false,
      uploadPercentage:  0,
      videos: [],
      edit_id: '',
      edit_mode: false,
    }
  },
  mounted() {
    this.get_video_details();
  },
  methods: {
    handleFileChange() {
      this.file = this.$refs.file.files[0];
    },
    removeFile() {
      // this.file.splice( 0, 1 );
      this.file = '';
    },
    videoLink(sub) {
      return `http:${app_domain}/${sub}`
    },
    resetForm() {
      this.file = '';
      this.title = '';
      this.loading= false;
      this.formErrors= {};
      this.success= false;
      this.error= false;
      this.uploadPercentage=  0;
      this.edit_mode = false;
      this.edit_id= '';
    },
    handleSubmit() {
      let title_error = this.check_form_error('title')
      let file_error = this.check_form_error('file')
      console.log('title_error', title_error)
      console.log('file_error', file_error)
      if (title_error || file_error) {
        return;
      }
      let formData = new FormData();
      formData.append('file', this.file);
      formData.append('title', this.title);
      let video_post_url = `${app_domain}/json/post_video_details`;
      this.loading = true;
      axios.post(video_post_url, formData,
          {
            headers: {
                'Content-Type': 'multipart/form-data'
            },
            onUploadProgress: ( progressEvent ) => {
              this.uploadPercentage = parseInt( Math.round( ( progressEvent.loaded * 100 ) / progressEvent.total ) );
            }
          }
        ).then(response =>{
          console.log('response inside success', response)
          this.resetForm();
          this.get_video_details();
        })
        .catch(response => {
          console.log('error response', response)
        });
    },
    check_form_error(key) {
      if (!this[key]) {
        this.formErrors = {
          ...this.formErrors,
          [key]: true,
        }
        return true;
      }else {
        this.formErrors = {
          ...this.formErrors,
          [key]: false,
        }
        return false;
      }
    },

    edit_or_post() {
      // not using
      if ( this.check_form_error('exam')) {
        return;
      }
      this.loading = true;
      if (this.edit_mode) {
        this.update_video_item();
      }else {
        this.post_video_details();
      }
    },
    get_video_details () {
      axios.get(`${app_domain}/json/get_video_details`).then(response => {
        this.videos = response.data;
      })
    },
    post_video_details () {
      // not using

      let video_post_url = `${app_domain}/json/post_video_details`;
      axios.post(video_post_url, this.video).then(response => {
        console.log('response', response);
        this.loading = false;
        this.resetForm();
        this.get_video_details();
        if (response.data.error) {
          this.error = true;
        }else {
          this.success = true;
        }
      })

    },
    update_video_item () {
      // not using
      let video_update_url = `${app_domain}/json/update_video_item/${this.edit_id}`;
      axios.put(video_update_url, this.video).then(response => {
        console.log('response', response);
        this.loading = false;
        this.edit_mode = false;
        this.resetForm();
        this.get_video_details();
        if (response.data.error) {
          this.error = true;
        }else {
          this.success = true;
        }
      })
    },
    edit_video_item(id) {
      // not using
      let video = this.video_collection.find(video => video.id === id);
      this.video = {...video};
      this.edit_mode = true;
      this.edit_id = video.id;
    },
    delete_video_item(id) {
      axios.delete(`${app_domain}/json/delete_video_item/${id}`).then(response => {
        this.get_video_details();
      })
    },


  },


}

export default {
  data() {
    return {
      file: '',
      title: '',
      loading: false,
      formErrors: {},
      success: false,
      error: false,
      uploadPercentage:  0,
      videos: [],
      edit_id: '',
      edit_mode: false,
    }
  },
  mounted() {
    this.get_video_details();
  },
  methods: {
    handleFileChange() {
      this.file = this.$refs.file.files[0];
    },
    removeFile() {
      // this.file.splice( 0, 1 );
      this.file = '';
    },
    videoLink(sub) {
      return `http:${app_domain}/${sub}`
    },
    resetForm() {
      this.file = '';
      this.title = '';
      this.loading= false;
      this.formErrors= {};
      this.success= false;
      this.error= false;
      this.uploadPercentage=  0;
      this.edit_mode = false;
      this.edit_id= '';
    },
    handleSubmit() {
      let title_error = this.check_form_error('title')
      let file_error = this.check_form_error('file')
      console.log('title_error', title_error)
      console.log('file_error', file_error)
      if (title_error || file_error) {
        return;
      }
      let formData = new FormData();
      formData.append('file', this.file);
      formData.append('title', this.title);
      let video_post_url = `${app_domain}/json/post_video_details`;
      this.loading = true;
      axios.post(video_post_url, formData,
          {
            headers: {
                'Content-Type': 'multipart/form-data'
            },
            onUploadProgress: ( progressEvent ) => {
              this.uploadPercentage = parseInt( Math.round( ( progressEvent.loaded * 100 ) / progressEvent.total ) );
            }
          }
        ).then(response =>{
          console.log('response inside success', response)
          this.resetForm();
          this.get_video_details();
        })
        .catch(response => {
          console.log('error response', response)
        });
    },
    check_form_error(key) {
      if (!this[key]) {
        this.formErrors = {
          ...this.formErrors,
          [key]: true,
        }
        return true;
      }else {
        this.formErrors = {
          ...this.formErrors,
          [key]: false,
        }
        return false;
      }
    },

    edit_or_post() {
      // not using
      if ( this.check_form_error('exam')) {
        return;
      }
      this.loading = true;
      if (this.edit_mode) {
        this.update_video_item();
      }else {
        this.post_video_details();
      }
    },
    get_video_details () {
      axios.get(`${app_domain}/json/get_video_details`).then(response => {
        this.videos = response.data;
      })
    },
    post_video_details () {
      // not using

      let video_post_url = `${app_domain}/json/post_video_details`;
      axios.post(video_post_url, this.video).then(response => {
        console.log('response', response);
        this.loading = false;
        this.resetForm();
        this.get_video_details();
        if (response.data.error) {
          this.error = true;
        }else {
          this.success = true;
        }
      })

    },
    update_video_item () {
      // not using
      let video_update_url = `${app_domain}/json/update_video_item/${this.edit_id}`;
      axios.put(video_update_url, this.video).then(response => {
        console.log('response', response);
        this.loading = false;
        this.edit_mode = false;
        this.resetForm();
        this.get_video_details();
        if (response.data.error) {
          this.error = true;
        }else {
          this.success = true;
        }
      })
    },
    edit_video_item(id) {
      // not using
      let video = this.video_collection.find(video => video.id === id);
      this.video = {...video};
      this.edit_mode = true;
      this.edit_id = video.id;
    },
    delete_video_item(id) {
      axios.delete(`${app_domain}/json/delete_video_item/${id}`).then(response => {
        this.get_video_details();
      })
    },


  },


}

~~~


{{< fd "Video.vue css part" >}}

~~~css
  input#image_upload {
  width: 0.1px;
  height: 0.1px;
  opacity: 0;
  overflow: hidden;
  position: absolute;
  z-index: -1;
}
.image-div {}
input#image_upload + label {
    font-size: 1.25em;
    cursor: pointer;
    font-weight: 700;
    color: #005da2;
    padding: 5px;
    background-color: transparent;
    border: 1px solid #c8c8c8;
    width: 100%;
    display: block;
    text-align: center;
}

input#image_upload:focus + label {}
input#image_upload + label:hover {
    background-color: #c8c8c8;
    color: white;
}
#image_upload + label * {
  pointer-events: none;
}
button.submit {
 background: #379e01;
 color: white;
 border: none;
 padding: 5px 10px;
 outline: none;
}

#selectedFiles li {
  color: gray;
  font-size: 14px;
}

#selectedFiles li span{
  padding-left: 20px;
  cursor: pointer;
  display: inline-block;
}
#selectedFiles li span:hover{
  color: tomato;
}
.span_times{
  color: #e43226;
  cursor: pointer;
    font-size: 23px;
    position: relative;
    top: 4px;
    left: 4px;
    }

~~~


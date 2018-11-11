+++
type="post"
toc=true
title= "Ajax Api Call for Multiple File Upload Using Ajax"
date= 2018-11-12T01:23:53+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
tutorial_tags= ['ajax', 'multiple file upload', 'formdata']
tutorialTypes=['tutorials']
keyword= "multiple file upload using ajax"
description= "How to upload multiple file using ajax and formdata"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

~~~js

//=================================================
// input On Change
//=================================================
function fileOnSelect(e) {
    e.preventDefault();
    var files = e.target.files;
    for (var i = 0; i < files.length; i++) {
        var eachFile = files[i];
        var formData = new FormData();
        formData.append("file", eachFile);

        uploadThisFile(formData);

    }

}

//=================================================
// Upload Each File
//=================================================
function uploadThisFile(formData){
    $.ajax({
        type: "POST",
        url: ['URL HERE'], // Upload URL Here
        data: formData,
        processData: false,
        contentType: false,
        xhr: function () {
            var xhr = new window.XMLHttpRequest();
            xhr.upload.addEventListener("progress", function (evt) {
                if (evt.lengthComputable) {
                    //=================================================
                    // this is the upload progress in percentage
                    //=================================================
                    var percentComplete = (evt.loaded / evt.total) * 100;
                    console.log(percentComplete);
                }
            }, false);
            return xhr;
        },
        success: function (response) {
            //=================================================
            // Ajax return Response
            //=================================================
            console.log(response);
        }
    });
}
~~~

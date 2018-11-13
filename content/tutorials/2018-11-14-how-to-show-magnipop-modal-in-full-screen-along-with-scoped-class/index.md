+++
type="post"
toc=true
title= "How to Show Magnipop Modal in Full Screen Along With Scoped Class"
date= 2018-11-14T01:12:52+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
language=['php']
software=['']
tutorial_tags= ['js plugin', 'magnipop model', 'lightbox', 'modal']
tutorialTypes=['tutorials']
keyword= "js plugin, magnipop model, lightbox modal"
description= "In this tutorial I have showed how to add magnipop modal in your website"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

# file dependency

* magnific-popup.css
* magnific-popup.js
* jquery

# Html part

~~~html
<div class='mainsingle__content__image'>
  <ul>
    <li>
      <a class='single-image-group' href='assets/images/product_image/0518254500_1_1_1.jpg'> <img src='assets/images/product_image/0518254500_1_1_1.jpg' alt=''/> </a>
      <a class='single-image-group' href='assets/images/product_image/0518254500_2_1_1.jpg'> <img src='assets/images/product_image/0518254500_2_1_1.jpg' alt=''/> </a>
      <a class='single-image-group' href='assets/images/product_image/0518254500_2_2_1.jpg'> <img src='assets/images/product_image/0518254500_2_2_1.jpg' alt=''/> </a>
      <a class='single-image-group' href='assets/images/product_image/0518254500_2_3_1.jpg'> <img src='assets/images/product_image/0518254500_2_3_1.jpg' alt=''/> </a>
      <a class='single-image-group' href='assets/images/product_image/0518254500_2_4_1.jpg'> <img src='assets/images/product_image/0518254500_2_4_1.jpg' alt=''/> </a>
      <a class='single-image-group' href='assets/images/product_image/0518254500_2_5_1.jpg'> <img src='assets/images/product_image/0518254500_2_5_1.jpg' alt=''/> </a>
      <a class='single-image-group' href='assets/images/product_image/0518254500_2_6_1.jpg'> <img src='assets/images/product_image/0518254500_2_6_1.jpg' alt=''/> </a>
    </li>
  </ul>
</div>
~~~

# css part
all css scoped under  `.singleproduct` by passing ` mainClass: 'singleproduct',` in js when initialize magnific-popup

~~~css
.singleproduct {
  .mfp-img {
   cursor: pointer;
  }
  .mfp-force-scrollbars {
    &.mfp-wrap {
        overflow-y: auto !important;
        overflow-x: auto !important;
    }
    .mfp-img {
      max-width: none;
    }
    .mfp-close {
     position: fixed;
    }

  }
}
~~~


# js part

~~~js
$(document).ready(function() {
  $('.mainsingle__content__image').magnificPopup({
    delegate: 'a.single-image-group',
    type: 'image',
    tLoading: 'Loading image #%curr%...',
    mainClass: 'singleproduct',
    gallery: {
      enabled: true,
      navigateByImgClick: true,
      preload: [0,1] // Will preload 0 - before current, and 1 after the current image
    },
    callbacks: {
      open: function() {
        var self = this;
        self.wrap.on('click.pinhandler', 'img', function() {
          self.wrap.toggleClass('mfp-force-scrollbars');
        });
      },
      beforeClose: function() {
            this.wrap.off('click.pinhandler');
        this.wrap.removeClass('mfp-force-scrollbars');
      }
    },

    image: {
      verticalFit: false
    }
  });

});
~~~

# we can use magnific-popup as modal plugin as well

## html part
~~~html
<a class="fs75 popup-modal"  href="#size_customization_modal">
  What's my size? <i class="fas fa-cut"></i>
</a>
<div id='size_customization_modal' class="white-popup-block mfp-hide">
  <h1>Size Modal - will be placed appropriate content later</h1>
  <p>You won't be able to dismiss this by usual means (escape or
    click button), but you can close it programatically based on
    user choices or actions.</p>
  <p><a class="popup-modal-dismiss" href="#">Dismiss</a></p>
</div>
~~~

## css part
~~~css

.white-popup-block  {
    background: #FFF;
    padding: 20px 30px;
    text-align: left;
    max-width: 650px;
    margin: 40px auto;
    position: relative;
}

~~~
## js part
~~~js
$('.popup-modal').magnificPopup();
~~~

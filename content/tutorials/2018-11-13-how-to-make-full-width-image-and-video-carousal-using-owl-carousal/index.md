+++
type="post"
toc=true
title= "How to Make Full Width Image and Video Carousal Using Owl Carousal"
date= 2018-11-13T19:38:42+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
language=['js']
software=['']
tutorial_tags= ['owl carousal 2', 'full width slider', 'video slider', 'carousal', 'js plugin']
tutorialTypes=['tutorials']
keyword= "owl carousal 2, full width slider, video slider, carousal"
description= "How to make a full width video and image slider using owl carousal"
skill_level=["intermediate"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++


# file dependency

~~~bash
https://cdnjs.cloudflare.com/ajax/libs/OwlCarousel2/2.3.4/assets/owl.carousel.min.css
~~~

~~~bash
https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js
https://cdnjs.cloudflare.com/ajax/libs/OwlCarousel2/2.3.4/owl.carousel.min.js
~~~

# html part

in html markup `.oc__item__content__media` accept 4 data attribute

* data-type='image'
* data-menu_color="black"
* data-background_position="center 0"
* data-source='assets/images/product_image/slider/desktop_background.jpg'

1. Here data-type will be `image` or `video`
2. `data-menu_color` will be `white` or nothing. I mean if you don't want to change basic black color, you can ignore.
3. `data-background_position` will be valid css background position - default `center center`. You can ignore if you want to make position `center center`
4. `data-source` will be image or video source. you have to pass relevant value for `data-type`


~~~html
<div class='mainhome__content'>
  <div class="owl-carousel">
    <div class='oc__item'>
      <div class='oc__item__content'>
        <div
          class='oc__item__content__media'
          data-type='video'
          data-menu_color="white"
          data-source='https://www.w3schools.com/howto/rain.mp4'
          >
        </div>
        <div class='oc__item__content__text'>
          <h2>carousal inner content 1</h2>
        </div>
      </div>
    </div>
    <div class='oc__item'>
      <div class='oc__item__content'>
        <div
          class='oc__item__content__media'
          data-type='image'
          data-menu_color="black"
          data-source='https://s3-us-west-2.amazonaws.com/s.cdpn.io/162656/owlcarousel2.jpg'
          >
        </div>
        <div class='oc__item__content__text'>
          <h2>carousal inner content 2</h2>
        </div>
      </div>
    </div>

    <div class='oc__item'>
      <div class='oc__item__content'>
        <div
          class='oc__item__content__media'
          data-type='image'
          data-menu_color="black"
          data-source='https://s3-us-west-2.amazonaws.com/s.cdpn.io/162656/owlcarousel1.jpg'
          >
        </div>
        <div class='oc__item__content__text'>
          <h2>carousal inner content 3</h2>
        </div>
      </div>
    </div>
    <div class='oc__item'>
      <div class='oc__item__content'>
        <div
          class='oc__item__content__media'
          data-type='video'
          data-menu_color="white"
          data-source='https://www.w3schools.com/howto/rain.mp4'
          >
        </div>
        <div class='oc__item__content__text'>
          <h2>carousal inner content 4</h2>
        </div>
      </div>
    </div>
    <div class='oc__item'>
      <div class='oc__item__content'>
        <div
          class='oc__item__content__media'
          data-type='image'
          data-menu_color="black"
          data-source='https://s3-us-west-2.amazonaws.com/s.cdpn.io/162656/owlcarousel3.jpg'
          >
        </div>
        <div class='oc__item__content__text'>
          <h2>carousal inner content 5</h2>
        </div>
      </div>
    </div>
  </div>
</div>
~~~


<div class="codepen">
  <h2>
      Embed from codepen
  </h2>
  <p data-height="400" data-theme-id="light" data-slug-hash="YRNrxO" data-default-tab="html,result" data-user="polo-dev-shibu" data-pen-title="How to Make Full Width Image and Video Carousal Using Owl Carousal" class="codepen">See the Pen <a href="https://codepen.io/polo-dev-shibu/pen/YRNrxO/">How to Make Full Width Image and Video Carousal Using Owl Carousal</a> by Polo Dev Shibu (<a href="https://codepen.io/polo-dev-shibu">@polo-dev-shibu</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
</div>


# scss part - You can view compile css in codepen

~~~css
body, html {
 margin: 0;
 padding: 0;
}
.owl-carousel {

}
.oc__item {
  background: rgba(0, 0, 0, .5);
  a {
    text-decoration: none;
    color: #333;
    display: block;
  }
  &__content {
    width: 100vw;
    height: 120vh;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    &__media {
      position: absolute;
      background-size: 'cover';
      background-position: center center;
      left: 0;
      right: 0;
      width: 100%;
      height: 100%;
      video {
        object-fit: fill;
      }
    }
    &__text {
      z-index: 2;
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100%;
      height: 100%;
    }
  }
}
~~~

# js part

~~~js
$(document).ready(function(){

// setup initial carousal item wallpaper on initialization event
$(".owl-carousel").on("initialized.owl.carousel", () => {
  console.log('initialization')
  setTimeout(() => {
   const $currentOwlMedia = $(".owl-item.active").find(".oc__item__content__media");
   console.log('$currentOwlMedia', $currentOwlMedia);
    callImageAndVideoMount($currentOwlMedia);
  }, 200);
});


// initialization
const $owlCarousel = $(".owl-carousel").owlCarousel({
  loop:true,
  items:1,
  dots: false,
});

// changed listener
$owlCarousel.on("changed.owl.carousel", e => {
  getCurrentOwlItem(e.item.index);
});


function mountImage ($element) {
  var source = $element.data('source')
  var background_position = $element.data('background_position')
  console.log('background_position', background_position)
  $element.css({
    'background-image': `url(${source})`
  })
  if (background_position) {
    $element.css({
      'background-position': background_position
    })
  }
}
function mountVideo($element) {
  var source = $element.data('source')
  var video = document.createElement('video');
  video.src = source;
  video.autoplay = true;
  video.loop = true;
  video.height = $element.height()
  video.width = $element.width()
  $element.html(video);
}

function getCurrentOwlItem (index) {
  const $currentOwlItem = $(".oc__item").eq(index);
  const $currentOwlMedia = $currentOwlItem.find('.oc__item__content__media');
  callImageAndVideoMount($currentOwlMedia);
}
function callImageAndVideoMount($currentOwlMedia) {
  var type = $currentOwlMedia.data('type')
  var menu_color =  $currentOwlMedia.data('menu_color')
  if (type === 'image') {
    mountImage($currentOwlMedia)
  }else if (type === 'video') {
    mountVideo($currentOwlMedia)
  }
  $body = $('body');
  if (menu_color === 'white') {
    $body.addClass('white_menu')
  }else {
    $body.removeClass('white_menu')
  }
}

});
~~~

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
tutorial_tags= ['owl carousal 2', 'full width slider', 'video slider', 'carousal']
tutorialTypes=['tutorials']
keyword= "owl carousal 2, full width slider, video slider, carousal"
description= "How to make a full width video and image slider using owl carousal"
skill_level=["intermediate"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

# html part

~~~html
<div class='mainhome__content'>
  <div class="owl-carousel">
    <div class='oc__item'>
      <div class='oc__item__content'>
        <div
          class='oc__item__content__media'
          data-type='image'
          data-menu_color="white"
          data-background_position="center 0"
          data-source='assets/images/product_image/slider/desktop_background.jpg'
          >
        </div>
        <div class='oc__item__content__text'>
          <h2 style="padding: 300px">Carousel One - inner content is totally in your control. whate ever design you want make, you can makeit. can add link </h2>
        </div>
      </div>
    </div>
    <div class='oc__item'>
      <div class='oc__item__content'>
        <div
          class='oc__item__content__media'
          data-type='video'
          data-menu_color="white"
          data-source='https://static.zara.net/video//mkt/2018/11/aw18-party-kids-video03d/aw18-party-kids-video03d_1.mp4'
          >
        </div>
        <div class='oc__item__content__text'>
          <!-- <h2 style="padding: 300px">Carousel Three - inner content is totally in your control. whate ever design you want make, you can makeit. can add link </h2> -->
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
          <h2 style="padding: 300px">Carousel Two - inner content is totally in your control. whate ever design you want make, you can makeit. can add link </h2>
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
          <h2 style="padding: 300px">Carousel Four - inner content is totally in your control. whate ever design you want make, you can makeit. can add link </h2>
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
          <h2 style="padding: 300px">Carousel five - inner content is totally in your control. whate ever design you want make, you can makeit. can add link </h2>
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
          <h2 style="padding: 300px">Carousel 6 - inner content is totally in your control. whate ever design you want make, you can makeit. can add link </h2>
        </div>
      </div>
    </div>
  </div>
</div>
~~~

# scss part

~~~css
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

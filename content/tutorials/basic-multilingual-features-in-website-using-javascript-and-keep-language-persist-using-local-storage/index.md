+++
type="post"
toc=true
title= "Basic Multilingual Features in Website Using Javascript and Keep Language Persist Using Local Storage"
date= 2018-11-12T01:23:52+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
tutorial_tags= ['javascript', 'multilingual']
tutorialTypes=['tutorials']
keyword= "language, multilingual"
description= "How to convert basic site into multilingual site"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

following snippet helps me to convert a basic site to 2 language site

## Html code for menu
~~~html
<h3
data-langen="Sorry this resturent does not have any menu yet . please try again later"
data-langhe="מצטערים, אין תפריט זה עדיין. בבקשה נסה שוב מאוחר יותר"
class="blanker">Sorry this resturent does not have any menu yet . please try again later </h3>

~~~

## initial code for checking country (in my case Israel) and changed language to Hebrew
~~~js
function changeLanguage (lang) {
    if (lang == 'en') {
        $("[data-langen]").each(function (index , val) {
            $(this).html($(this).attr('data-langen'));
        });
    } else {
        $("[data-langhe]").each(function (index , val) {
            $(this).html($(this).attr('data-langhe'));
        });
    }
}
if (localStorage.getItem('user_lang')) {
  let lang = localStorage.getItem('user_lang');
  changeLanguage(lang);
} else {
  $.get('http://ip-api.com/json', (response) => {
    if (response.countryCode == "IL") {
      localStorage.setItem("user_lang", 'il')
      changeLanguage('he');
    }
  })
}
~~~

## language icon for changing language manually

~~~html
{% raw %}
<div class="language-changer">
  <p><a href="<?= (@end(explode('@', request()->route()->getAction('controller'))) == 'catmenu') ? action('frontEndController@language', ['lan' => 'en']) : '#'; ?>" class="en"><img src="{{ asset('img/en.png') }}" alt="Language icon is not available"></a></p>
  <p>
    <a href="<?= (@end(explode('@', request()->route()->getAction('controller'))) == 'catmenu') ? action('frontEndController@language', ['lan' => 'he']) : '#'; ?>" class="il"><img src="{{ asset('img/il.png') }}" alt="Language icon is not available"></a>
  </p>
</div>
{% endraw %}
~~~

## click event when user click on language changing button

~~~js
$(".language-changer  a").on('click' , function (event) {
    if ($(this).attr('href') == '#') {
        event.preventDefault();
        var lang = $(this).attr('class');
        localStorage.setItem("user_lang", lang)
        changeLanguage(lang)
    }
});
~~~



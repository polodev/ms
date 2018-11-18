+++
type="post"
toc=true
title= "Sibling Communication in Vue Js Using Event Bus"
date= 2018-11-18T22:27:50+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
language=['php']
software=['']
tutorial_tags= ['vue js', 'vue js events', 'sibling communication']
tutorialTypes=['tutorials']
keyword= "vue js, vue js events, sibling communication"
description= "In this tutorial I have shown how to make sibling relation ship"
skill_level=["beginner"]
available_skill_level=["intermediate",]
series=[]
series_weight=1
+++

I wanted to make a single page application using vue. Functionality will be like following,
when you click on sidebar link respective component will be load. Like following design I need 4 main component.

* Main component
* Sidebar component
* home component
* about component
* contact component


~~~js
-------------------------------------
home    |     content of respective tab
        |
about   |
        |
contact |
        |
-------------------------------------
~~~

In main.vue component, sidebar component and a component from (home.vue, about.vue, contact.vue) will be mount based on sidebar state. This functionality we
can achieve through Event bus

following entry js file for calling vue component, instantiate vue for main app and event bus.

<p class="file-desc">
  <span>entry.js</span>
</p>

~~~js
window.Vue = require('vue');
window.SidebarEvent = new Vue();
const app = new Vue({
    el: '#app',
    store
});
~~~

Here I instantiate vue and assign to `window.SidebarEvent` for using this instance as event bus. And instantiate Vue for main app as well

require all 5 component in entry js file

<p class="file-desc">
  <span>entry.js</span>
</p>

~~~js
window.Vue = require('vue');
window.SidebarEvent = new Vue();

Vue.component('app-main', require('./components/main.vue'));
Vue.component('app-sidebar', require('./components/sidebar.vue'));
Vue.component('app-home', require('./components/home.vue'));
Vue.component('app-about', require('./components/about.vue'));
Vue.component('app-contact', require('./components/contact.vue'));

const app = new Vue({
    el: '#app',
    store
});
~~~

# Writing on main.vue component

<p class="file-desc">
  <span>main.vue html part</span>
</p>

~~~html
<template>
  <transition name="fade">
    <div class='container'>
      <div class='row'>

        <div class='col-md-3'>
          <app-sidebar></app-sidebar>
        </div>

        <div class='col-md-9'>
          <app-home v-if="isSelect('home')" ></app-home>
          <app-about v-if="isSelect('about')" ></app-about>
          <app-contact v-if="isSelect('contact')" ></app-contact>
        </div>

      </div>
    </div>
  </transition>
</template>
~~~

<p class="file-desc">
  <span>main.vue js part</span>
</p>

~~~js
export default {
  mounted() {
    SidebarEvent.$on('sidebarchanged', (data) => {
      this.selectedSidebar = data.selectedSidebar;
    });
  },
  data: function () {
    return {
      selectedSidebar: 'home',
    }
  },
  methods: {
    isSelect(selectedSidebar) {
      return this.selectedSidebar == selectedSidebar;
    }
  }

}
~~~

In main component we decided which component is active using `isSelect()` method inside `v-if` directive.
Based on this we show component. inside `data` method we return default selectedSidebar is `home`.
In mounted method we are constantly listen to a event called `sidebarchanged` using `SidebarEvent.$on()` function. those
`sidebarchanged` event will triggered from `sidebar component`

# sidebar.vue component

<p class="file-desc">
  <span>sidebar.vue html part</span>
</p>

~~~html
<template>
  <ul>
    <li @click="changeSidebar('home')" :class="{'active': isSelectedSidebar('home')}">Home</li>
    <li @click="changeSidebar('about')" :class="{'active': isSelectedSidebar('about')}">About</li>
    <li @click="changeSidebar('contact')" :class="{'active': isSelectedSidebar('contact')}">Contact</li>
  </ul>
</template>

~~~

<p class="file-desc">
  <span>sidebar.vue js-part</span>
</p>

~~~js
export default {
  data() {
    return {
      selectedSidebar: 'personal',
    }
  },
  methods: {
    isSelectedSidebar(selectedSidebar) {
      return selectedSidebar == this.selectedSidebar;
    },
    changeSidebar(selectedGroup, selectedSidebar) {
      this.selectedSidebar = selectedSidebar;
      SidebarEvent.$emit('sidebarchanged', {
        selectedSidebar,
      })
    }
  },
}
~~~

In side bar component using `changeSidebar`, we change sidebar state. Once we click on `changeSidebar` it will emit 'sidebarchanged'
event using `SidebarEvent.$emit()` function. this emit will be receive by Main component `SidebarEvent.$on()` function.
`$emit()` function will take 2 parameter. first one is event name (in our case `sidebarchanged`) 2nd one is value we want to pass. In this case we pass
value like `home` `about` `contact` to track our state


# home.vue, about.vue, contact.vue component

~~~html
<template>
  <transition name="fade">
    content of home or about or contact
  </transition>
</template>
~~~

# Transition in vue

Here I have added a transition name with fade. To get those transition we have add following css in our css file

~~~css
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
~~~



Thanks

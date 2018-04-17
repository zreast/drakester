<template>
  <div id="app">
    <router-view></router-view>
    <vue-progress-bar></vue-progress-bar>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Animate from 'animate.css/animate.css'
  import VueResource from 'vue-resource'
  import Cookie from 'js-cookie'
  import MeterialIcons from 'material-design-icons'
  import 'material-design-icons/iconfont/material-icons.css'

  Vue.use(Animate)
  Vue.use(VueResource)
  Vue.use(MeterialIcons)
  Vue.http.headers.common['X-CSRFToken'] = Cookie.get('csrftoken')

  export default {
    name: 'app',
    created () {
      this.$Progress.start()
      this.$router.beforeEach((to, from, next) => {
        if (to.meta.progress !== undefined) {
          let meta = to.meta.progress
          this.$Progress.parseMeta(meta)
        }
        this.$Progress.start()
        next()
      })
      this.$router.afterEach((to, from) => {
        this.$Progress.finish()
      })
      setTimeout(() => {
        $('#paw-loading').css('display', 'none')
      }, 1000)
    }
  }
</script>

<style lang="scss">
  @import url(https://fonts.googleapis.com/css?family=Prompt);
	@import "css/colors.scss";

  $form-theme-color: #49392C;

	$maincolor:#FFF;
	$gray:#ECF0F1;
	$yellow_alart:#fff6bf;
	$green_alart:#e6efc2;
	$green:#82C108;
	$gray_fade:#ede4eb;
	$gray_fade2:#cbc7ce;
	$dark_gray:#9c9c9c;
	$dark_gray2:#747577;
	$main_purple:#9e13ba;
	$subcolor:#FFF;
	$blue:#019cff;
	$sub2color:#FFF;
	$header_color:#efe8df;
	$google_color:#D73D32;
	$facebook_color:#4E69A2;
	$twitter_color:#55ACEE;
	$niyay_color:$green;
	$almost_black:#252525;

	// Main Theme Color
	$orenge_soft:#ffc98c;
	$orenge_soft2:#ffdfba;
	$orenge_theme:#FFAA4D;
	$brown:#b38732;
	$almost_white:#f2f2f2;
	$almost_white2:#f5f5f5;
	$material_gray_lighter:#BBBBBB;
	$material_gray_lighter_2:#D5D5D5;
	$material_gray:#555555;
	$material_gray_darker:#4c4c4c;
	$material_dark:#363636;

  body, html {
    font-family: 'Prompt',sans-serif;
  }

	.font_prompt{
		font-family: 'Prompt',sans-serif;
	}

</style>

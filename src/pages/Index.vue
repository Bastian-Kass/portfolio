<template>
  <div id="home">

    <!-- <div id="app_header"  :style="menu_height">
      <q-toolbar :style="menu_height">

        <q-toolbar-title>
            <span class="header_title noselect" style="margin: 0;">
              Sebastian Casillas
            </span>
        </q-toolbar-title>

        <q-tabs shrink class="index_navigation_tabs">
            <q-route-tab
                label="Workflow"
                to="/"
                exact
                />

            <q-route-tab
                label="Background"
                to="/background"
                exact
                />

            <q-route-tab
                label="Projects"
                to="/portfolio"
                exact
                />

        </q-tabs>
      </q-toolbar>
    </div> -->

    <q-scroll-area id="home_main" ref="home_main" style="padding:0" :visible="false">
      <div id="contact_component" class="full-width column items-center">
        
        <q-space style="height: 10vh"></q-space>
        
        <div class="q-ma-xl">
          <contact-card/>
        </div>

        <q-space style="height: 10vh"></q-space>
        
        

        <q-tabs shrink class="index_navigation_tabs">
          <q-route-tab
              label="Workflow"
              to="/"
              exact
              />

          <q-route-tab
              label="Background"
              to="/background"
              exact
              />

          <q-route-tab
              label="Projects"
              to="/portfolio"
              exact
              />

        </q-tabs>

        <div style="max-width: 1200px; width: 100%;">
          <router-view v-slot="{ Component }">
            <transition name="slide-fade">
              <component :is="Component" />
            </transition>
          </router-view>
        </div>


      </div>
    </q-scroll-area>

  </div>


</template>

<script>

import ContactCard from '@/components/home/ContactCard.vue'

import CV from '@components/cv/cv.vue'
import WordCloud from '@components/home/WordCloud.vue'
import ProjectView from '@/components/portfolio/ProjectView.vue'

import { ref } from 'vue'

export default {
  name: 'Home',
  components: {
    cv: CV,
    ContactCard, 
    ProjectView,
    WordCloud
  },

  data: () => ({
    v_scroll: 0,
    menu: true,
    cv_dialog: false,
    project_show: null,
  }),

  setup(){
      const percent = ref(0);

      const thresholds = []
      for (let i = 0; i <= 1.0; i += 0.05) 
        thresholds.push(i)
      
      return {
              percent,
              options: {
                  handler (entry) {
                      const val = (entry.intersectionRatio * 100).toFixed(0)
                      if (percent.value !== val) percent.value = val
                  },
                  cfg: {threshold: thresholds}
              }
          }
  },



  computed:{
    percent_u: function(){
      return this.percent / 100;
    },
    menu_height: function(){
      let val = (40 + 40 * this.percent/100).toString()
      return  `height:  ${val}px; min-height: ${val}px; `
    },

    scrollbased_transparency: function(){
      let val = (254* this.percent/100).toString()
      return  `background-color: rgba(68, 85,102, ${val});`
    },

    max_width: function(){
      let calc_width = (740 + this.percent_u * 60).toString()

      return 'width:' + calc_width + 'px;';
    },
    is_project_selected: function(){
      return this.project_show !== null
    }
  },

  methods: {
    select_project(project){
      this.project_show = project;
    },
    hide_project_dialog(){
      this.project_show = null
    }
  }
}

</script>

<style lang="scss">

  #home{
    width: 100vw;
    max-width: 100vw;
    height: 100vh;
    overflow: hidden;
  }

  #home_main{
    max-width: 100%;
    overflow: none;
  }

  #app_header{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;

    backdrop-filter: blur(4px);

    background-color: #2223;

    display:flex;
    flex-wrap: wrap;
    justify-content: center;

    z-index: 5;

    & > * {
      max-width: 1000px;
      border-bottom: solid 1px #FFFA;
    }


    .header_title{
      font-size: 1.2em;
    }

  }
  
  #home_main{
    height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: column;
    align-items: center;

  } 
  
  .index_navigation_tabs{
    height: 100%;

    .q-tab__content, .q-tab{
      height: 100%;
      min-height: 100%;
    }
  }

  .q-dialog__inner{
    overflow:hidden;
  }


  .slide-fade-enter-active {
    transition: all .3s ease;
  }
  .slide-fade-leave-active {
    transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-fade-enter, .slide-fade-leave-to
  /* .slide-fade-leave-active below version 2.1.8 */ {
    transform: translateX(10px);
    opacity: 0;
  }




</style>
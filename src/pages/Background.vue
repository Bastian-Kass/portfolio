<template>

    <div id="background_div" class="fill-width">

        <div class="row justify-between items-center">
            <div>
                <h4 class="items-center" style="color: #000;">Background</h4>
            </div>

            <q-space></q-space>
            
            <div>
                <span class="text-black q-mr-sm">
                    Download my 
                </span>
                <q-btn round color="primary" :size="'20px'">
                CV
                </q-btn>
            </div>
            
        </div>
        

        <div class="q-px-lg q-pb-md">
            <q-timeline color="secondary" :dark="false" :layout="layout">

                <background-card v-for="(value, name) of background" :key="name" :data="value" />

            </q-timeline>
        </div>
        
    </div>
</template>

<script>

import BackgroundCard from '@/components/home/BackgroundCard.vue'

export default {
    name: 'MyComponent',
    components:{BackgroundCard},
    data: () => ({
        background: [],
    }),
    created() {

        this.$api
            .post('collections/get/background')
            .then( res => res.data?.entries )
            .then(d => {  this.background = d; })
            .catch( e => console.log(e));

  },
    computed:{
        layout: function () {
            return this.$q.screen.lt.sm ? 'dense' : (this.$q.screen.lt.md ? 'comfortable' : 'loose')
        }
    }
}
</script>

<style lang="scss" scoped>
#background_div{
    padding-top: 1rem;
    padding-right: 3rem;
    padding-left: 3rem;
    background-color: white;

    .q-timeline.q-timeline--dark{

        & > .q-timeline__entry{
            color: #111 !important;

            .q-timeline__title{
                color: #111 !important;
                font-size: 2rem !important;
                line-height: 1.6rem !important;
                font-weight: 300;
            }

            .q-timeline__subtitle{
                color: #333 !important;
            }
        }


    }


}
</style>
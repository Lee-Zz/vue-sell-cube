<template>
  <div id="app">
    <v-header :seller='seller'></v-header>
    <v-tab></v-tab>
    <router-view :seller='seller'></router-view>
  

  </div>
</template>

<script>
import header from 'components/header/header'
import tab from 'components/tab/tab'
import star from 'components/star/star'
export default {
  name: 'App',
  data(){return{
    seller:{}
  }},
  created(){
    this.getHeadData()
  },
  methods:{
    getHeadData(){
      this.$http.get('/api/seller').then(res=>{
        if(res.body.errno == 0){
          this.seller = res.body.data
        }
      })
    }
  },
  components: {
    'v-header': header,
    'v-tab':tab,
    'v-star':star
  }
}
</script>

<style lang="stylus">

</style>

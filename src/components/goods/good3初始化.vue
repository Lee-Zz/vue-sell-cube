<template>
  <div class="goods">
    
    <div class="good_left" >
        <div v-for="(item,index) in foodData" :key="index" class="good_left_box">
            {{item.name}}
        </div>
    </div>
  
    <div class="good_right" >
        <div v-for="(item, index) in foodData" :key="index">
            <div class="good_title">
            {{item.name}}
            </div>

            <div class="good_content" v-for="(item,index) in item.foods" :key="index">
            <div class="good_content_left">
                <img :src="item.image"/>
            </div>
            <div class="good_content_right">
                <div class="good_content_title">
                {{item.name}}
                </div>
                <div class="good_content_2">
                <div class="good_content_2_1">
                    月售{{item.sellCount}}份
                </div>
                
                <div class="good_content_2_2">好评率{{item.rating}}%</div>
                </div>
                <div class="good_content_price">
                <div>{{item.price}}</div>
                <div>{{item.oldPrice}}</div>
                </div>
            </div>
            </div>

        </div>
    </div>
  
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from "better-scroll"
  export default {
  
    data() {
      return { 
        foodData:[],
 
      };
    },
 
    created(){
      this.$http.get('/api/goods').then((res)=>{
        res = res.body
        if(res.errno == 0){
          this.foodData = res.data
         
        }
      })
    },
    methods: {
     _initScroll(){
     
     },
     _calculateHeight(){
         
     }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .goods
    display:flex
    position: absolute
    top:146px
    bottom:64px
    overflow:hidden
    width:100%
    .good_left
      flex: 0 0 80px
      width: 80px
      background:#f3f5f7
      .good_left_box
        background #f3f5f7
        border-bottom:1px solid red
        height 54px
        &.current
          background red
    .good_right
      flex:1
      background:#fff
      .good_title
        width 100%
        height 26px
        background #f3f5f7
        border-left 2px #d9dde1 solid 
        font-size 12px
        color rgb(147,153,159)
        box-sizing border-box
        padding-left 14px
        line-height 26px
      .good_content
        padding 18px
        box-sizing border-box
        display flex
        justify-content flex-start
        .good_content_left
          width 64px
          height 64px
          border 1px solid red
          img
            width 100%
            height 64px
        .good_content_right
          flex 1
          height 100px
          border 1px solid red
          margin-left 10px
          .good_content_title
            font-size 14px
            color rgb(7,17,27)
            line-height 14px
          .good_content_2
            display flex
            justify-content flex-start
            .good_content_2_1
              font-size 10px
              color rgb(147,153,159)
              line-height 10px
            .good_content_2_2
              font-size 10px
              color rgb(147,153,159)
              line-height 10px
              margin-left 12px
</style>
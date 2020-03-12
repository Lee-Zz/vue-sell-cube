<template>
<div>
  <div class="goods" > 
    <div ref="scroll1" >

         <div class="good_left" >
            <div  v-for="(item,index) in foodData" :key="index" class="good_left_box" :class="{'current':currentIndex === index}" @click="selectMenu($event,index)" >
                {{item.name}}
            </div>
        </div>
    </div>
    <div ref="scroll2">
        <div class="good_right"> 
            <div v-for="(item, index) in foodData" :key="index" ref="scroll3">
                <div class="good_title" >
                {{item.name}}
                </div>
                <div class="good_content" v-for="(item,index) in item.foods" :key="index">
                  <div class="good_content_left" >
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
                      <cartcontrol @add="addFood" :food="item"></cartcontrol>
                  </div>
                </div>
            </div>
        </div>
    </div>
    <div>
    </div>
  </div>
    <v-cart ref="shopcart" :selectFoods="selectFoods" :minPrice="seller.minPrice" :deliveryPrice="seller.deliveryPrice"></v-cart>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from "better-scroll"
  import shopcart from 'components/shopcart/shopcart'
  import cartcontrol from 'components/cartcontrol/cartcontrol'
  export default {
  
    data() {
      return { 
        foodData:[],
        heightList:[],
        scrollY:0
      };
    },
 
    created(){
      this.$http.get('/api/goods').then((res)=>{
        res = res.body
        if(res.errno == 0){
          this.foodData = res.data
         
          this.$nextTick(()=>{
              this._initScroll()
              this.getHeightList()
          })
          
        }
      })
    },
    props:{
      seller:{
        type:Object
      }
    },
    computed:{
        currentIndex(){ 
            for(let i =0;i<this.heightList.length;i++){
                let height = this.heightList[i] 
                let height2 = this.heightList[i+1]
                if(!height2 || (this.scrollY >= height && this.scrollY < height2)){
                    return i   
                }
            }
            return 0
        },
        selectFoods(){
          let foods = []
          this.foodData.forEach((good)=>{
            good.foods.forEach((food)=>{
              if(food.count){
                foods.push(food)
              }
            })
          })
          return foods
        }
    },
    methods: {
        addFood(target){
          this._drop(target)
        },
        _drop(target){
          this.$nextTick(()=>{
            this.$refs.shopcart.drop(target)
          })
        },
        selectMenu(e,id){
          if(!e._constructed){   return}
          let el = this.$refs.scroll3[id]
          this.footScroll.scrollToElement(el,300)
        },
     _initScroll(){
        this.menuScroll = new BScroll(this.$refs.scroll1,{
            click: true
        })
        this.footScroll = new BScroll(this.$refs.scroll2,{
            probeType:3,
            click: true
        })
        this.footScroll.on("scroll",(pop)=>{
            this.scrollY  = Math.abs(Math.round(pop.y)) 
        })
     },
     _calculateHeight(){

     },
     selefn(){
     
     },
     getHeightList(){
        let heightList = this.$refs.scroll3
        this.heightList.push(0)
        let height = 0
        heightList.forEach((item)=>{
            let itemHeight = item.clientHeight  //(元素的高度)
            height += itemHeight
            this.heightList.push(height)
        })
     }
    },
    components:{
      "v-cart":shopcart,
      cartcontrol
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
    z-index:-99
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
        // background #f3f5f7
        background red
        border-left 2px #d9dde1 solid 
        font-size 12px
        color rgb(147,153,159)
        box-sizing border-box
        padding-left 14px
        line-height 26px
        position: -webkit-sticky;
        position: sticky;
        top: 0;

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
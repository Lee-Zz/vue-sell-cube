<template>
  <div class="header" @click="detailShow = true">
    <div class="header_div">
      <div class="right">
        <img :src="seller.avatar"/>
      </div>
      <div class="left">
        <div class="title">
          <span class="brand"></span>
          <div class="title_text">{{seller.name}}</div>
        </div>
        <div class="content">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="left_bottom">
            <div class="active_box">
              <span class="icon" :class="classMap[seller.supports[0].type]"></span>
              <div class='active_box_text'>{{seller.supports[0].description}}</div>
            </div>
            <div class="btn_box">
              <div class="btn_text">{{seller.supports.length}}个</div>
              <span class="right_icon"> > </span>
            </div>
        </div>
      </div>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <div v-show="detailShow" class="pc_div">
       <div class="detailTitle">{{seller.name}}</div>
       <div class='starbox'>
         <v-star :size='48' :count='3.2'></v-star>
       </div>

      <div class="yhxx_div">
        <div class="yhxx_div1"></div>
        <div class="yhxx_div2">优惠信息</div>
        <div class="yhxx_div3"></div>
      </div>

      <div class="ac_box">
        <div class="ac_box_box" v-for="item in seller.supports" :key="item.type">
          <span class="ac_box_com" :class="classMap[item.type]"></span>
          <div class='ac_box_text'>{{item.description}}</div>
        </div>
      </div>
      <div class="yhxx_div">
        <div class="yhxx_div1"></div>
        <div class="yhxx_div2">商家公告</div>
        <div class="yhxx_div3"></div>
      </div>

      <div class="textwby">
        {{seller.bulletin}}
      </div>
       
      <div class="close" @click.stop="detailShow = false">x</div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  import star from 'components/star/star'
  export default {
    data() {
      return { 
       detailShow: false
      };
    },
    props:{
      seller:{
        type:Object
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components:{
      "v-star":star
    }
  };
</script>  

<style lang="stylus" rel="stylesheet/stylus">
@import "common/stylus/mixin.styl"
  .header
    position: relative
    width:100%
    height:106px
    .background
      position: absolute
      top:0
      left:0
      width:100%
      height:100%
      z-index: -1
      filter: blur(10px)
    .pc_div
      width:100%
      height:100%
      position:fixed
      top:0
      left:0
      backdrop-filter: blur(10px)
      opacity: 1
      background: rgba(7, 17, 27, 0.8)
      padding:64px 36px
      box-sizing:border-box
      z-index :9999
      .detailTitle
        font-size:16px
        font-weight:bold
        color:#fff
        line-height:16px
     
        text-align:center
      .starbox
        display:flex;
        justify-content:center
        margin-top:16px
        height:24px
        
        
      .yhxx_div
        width :100%
        height: 14px
        margin-top :28px
        display:flex
        justify-content flex-start
        align-items:center
        .yhxx_div1
          width :100%
          height:2px
          background:rgba(255,255,255,0.2)
          flex :1
        .yhxx_div2
           font-size 14px
           font-weight:bold
           color:#fff
           height:14px
           line-height:14px
           margin:0 12px
           box-sizing:border-box
        .yhxx_div3
          width :100%
          height:2px
          background:rgba(255,255,255,0.2)
          flex :1
      .ac_box
        padding:24px 0 28px 0
        .ac_box_box
          height:16px
          display :flex
          justify-content flex-start
          margin-bottom:12px
          padding-left :12px
          box-sizing:border-box
          &:last-child
            margin-bottom :0
          .ac_box_com
            width:16px
            height:16px
            background-size:16px 16px
            background-repeat:no-repeat
            display:inline-block
            &.decrease
              bg-image("decrease_1")
            &.discount
              bg-image("discount_1")
            &.special
              bg-image("special_1")
            &.invoice
              bg-image("invoice_1")
            &.guarantee
              bg-image("guarantee_1")
          .ac_box_text
            height :16px
            line-height :16px
            font-size :12px
            color :#fff
            margin-left :6px
      .textwby
        font-size :12px
        color :#fff
        line-height :24px
        margin-top:24px
      .close
        position:absolute
        width:100%
        bottom:32px
        left 0
        margin:auto
        color:#fff
        text-align:center
        font-size:20px
    .header_div
      width:100%
      height:106px
      background:rgba(7,17,27,0.5)
      padding:24px 12px 18px 24px
      box-sizing:border-box
      display:flex
      justify-content:flex-start
      .right
          width:64px
          height:64px
          margin-right:16px
          img
            width:100%
            height:64px
        .left
          flex:1
          height:64px
          .title
            display:flex
            justify-content:flex-start
            align-items:center
            .brand
              width:30px
              height:18px
              display:inline-block
              bg-image('brand')
              background-size:30px 18px
              background-repeat:no-repeat
            .title_text
                font-size:16px
                color:#fff
                font-weight: bold
                line-height: 18px
                margin-left:6px
          .content
              font-size:12px
              color:#fff
              line-height:12px
              margin-top:8px
              font-weight:200
          .left_bottom
            margin-top:10px
            display:flex;
            justify-content:space-between
            align-items:center
            .active_box
              display:flex
              justify-content:flex-start
              align-items:center
              .active_box_text
                font-size:12px
                color:#fff
                font-weight:200
                line-height:12px
                margin-left:4px
              .icon
                display:inline-block
                width:12px 
                height:12px
                background-size:12px 12px
                background-repeat:no-repeat
                &.decrease
                  bg-image('decrease_1')
                &.discount
                  bg-image('discount_1')
                &.special
                  bg-image('special_1')
                &.invoice
                  bg-image('invoice_1')
                &.guarantee
                  bg-image('guarantee_1')

            .btn_box
              padding:0 8px
              box-sizing:border-box
              display:flex
              justify-content:flex-start
              background:rgba(0,0,0,.2)
              border-radius: 14px
              .btn_text
                line-height:24px
                height:24px
                font-size:10px
                color:#fff
              .right_icon
                margin-left: 2px
                line-height: 24px
                font-size: 10px
                color:#fff

</style>
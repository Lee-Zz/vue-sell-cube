<template>
    <div>
        <div class="bottom_box" @click.stop.prevent="toggleList">
            <div class="bottom_box_div">
                <div class="div1">
                    <div class="div1_div" :class="{'activeDiv':totalNum>0}">
                    <i class="icon-shopping_cart"></i>
                    </div>
                    <div v-show="totalNum > 0" class="reddain">{{totalNum}}</div>
                </div>
                <div class="jine">￥{{totalPrice}}</div>
                <div class="psf">另需配送费￥{{deliveryPrice}}元</div>
            </div>
            <div class="jiesuan" :class="{'activeJiesuan':totalPrice>=minPrice}">{{jiesuanText}}</div>
            <transition name="move">
                <div class="shopcart-list" v-show="showList">
                    <div class="shopcart-head">
                        <div class="shopcart-headtext">购物车</div>
                        <div class="shopcart-headtext headcolor">清空</div>
                    </div>
                    <div ref="shopcartList" v-on:click.stop="">
                        <ul class="shopcart-body" v-on:click.stop="">
                            <li v-for="(item,index) in selectFoods" :key="index" class="shopcart-item">
                                <div class="shopcart-item_left">
                                    {{item.name}}
                                </div>
                                <div class="shopcart-item_right">
                                    <div class="shopcart-item_rightText">
                                        ￥{{item.price}}
                                    </div>
                                    <div>
                                        <cartcontrol :food="item"  @add="addFood"></cartcontrol>
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </transition>
        </div>
        <div class="ball-container" >
            <div v-for="(ball,index) in balls" :key="index">
                <transition name="drop" @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
                    <div class="ball" v-show="ball.show">
                        <div class="inner inner-hook"></div>
                    </div>
                </transition>
            </div>
        </div>
    </div>
</template>
<script type='text/ecmascript-6'>
import cartcontrol from "components/cartcontrol/cartcontrol.vue"
import BScroll from 'better-scroll';
export default {
    data(){
        return {
            balls: [
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                }
            ],
            dropBalls:[],
            fold:true
        }
    },
    props: {
        selectFoods: {
            type: Array,
            default(){
                return []
            }
        },
        minPrice:{
            type: Number,
        },
        deliveryPrice:{
            type: Number,
        }
    },
    computed: {
        totalPrice(){
            let num = 0
            this.selectFoods.forEach(item=>{
                num += item.price * item.count   
            })
             return num
        },
        totalNum(){
            let goodNum = 0
            this.selectFoods.forEach(item=>{
                goodNum += item.count
            })
            return goodNum
        },
        jiesuanText(){
            if(this.totalPrice === 0){
                return  `￥${this.minPrice}元起送`
            }else if(this.totalPrice < this.minPrice){
                return `还差${this.minPrice - this.totalPrice}元起送`
            }else if(this.totalPrice >= this.minPrice){
                return '去结算'
            }
        },
        showList(){
            if(!this.totalNum){
                this.fold = true
                return false
            }
            let show = !this.fold
            if(show){
                console.log(this.$refs.shopcartList,'aaasdsad')
                this.$nextTick(()=>{
                    if(!this.scroll){
                        this.scroll = new BScroll(this.$refs.shopcartList,{
                            click: true
                        })
                    } else{
                        this.scroll.refresh();
                    }
                })
            }
            return show
        }
    },
    methods:{
        addFood(target){
             this.drop(target)
        },
       
        toggleList(e){
            if(!this.totalNum){
                    return
                }
                this.fold = !this.fold
        },
        drop(el){
            console.log(el,'el')
            for(let i = 0; i < this.balls.length;i++){
                let ball = this.balls[i]
                if(!ball.show){
                    ball.show = true
                    ball.el = el
                    this.dropBalls.push(ball)
                    return
                }
            }
            
        },
        beforeDrop(el){
            let count = this.balls.length;
            while (count--) {
                let ball = this.balls[count];
                if (ball.show) {
                    let rect = ball.el.getBoundingClientRect();
                    let x = rect.left - 32;
                    let y = -(window.innerHeight - rect.top - 22);
                    el.style.display = '';
                    el.style.webkitTransform = `translate3d(0,${y}px,0)`;
                    el.style.transform = `translate3d(0,${y}px,0)`;
                    let inner = el.getElementsByClassName('inner-hook')[0];
                    inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
                    inner.style.transform = `translate3d(${x}px,0,0)`;
                }
            }
        },
        dropping(el, done) {
            /* eslint-disable no-unused-vars */
            let rf = el.offsetHeight;
            this.$nextTick(() => {
                el.style.transform = 'translate3d(0,0,0)';
                let inner = el.getElementsByClassName('inner-hook')[0];
                inner.style.webkitTransform = 'translate3d(0,0,0)';
                inner.style.transform = 'translate3d(0,0,0)';
                el.addEventListener('transitionend', done);
            });
        },
        afterDrop(el){
            let ball = this.dropBalls.shift()
            if (ball) {
                ball.show = false;
                el.style.display = 'none';
            }
        }
    },
    components:{
        cartcontrol
    }
};
</script>
<style scoped lang='stylus' rel='stylesheet/stylus'>
.bottom_box 
  width: 100%;
  height: 48px;
  background: #141d27;
  position: absolute;
  bottom: 0;
  left: 0;
  padding-left: 12px;
  box-sizing: border-box;
  display flex
  justify-content space-between
  .shopcart-list
    width 100%
    background #fff
    height 281px
    position absolute
    left 0
    top 0
    z-index -1
    transform translate3d(0,-100%, 0)
    &.move-enter-active,&.move-leave-active
        transition: all 1s ease
    &.move-enter,&.move-leave-active
        transform translate3d(0,0,0)
    .shopcart-head
        width 100%
        height 40px
        background #f3f5f7
        border-bottom 1px solid rgba(7,17,27,0.1)
        padding 0 18px
        display flex
        justify-content space-between
        align-items center
        box-sizing border-box
        .shopcart-headtext
            font-size 14px
            color rgb(7,17,27)
            &.headcolor
                color rgb(0,160,200)
    .shopcart-body
        padding 0 18px
        box-sizing:border-box
        max-height: 217px
        overflow: hidden
        .shopcart-item
            height 48px
            border-bottom 1px solid rgba(7,17,27,0.1)
            display flex
            justify-content space-between
            .shopcart-item_left
                font-size 14px
                color rgb(7,17,27)
                line-height 48px
             .shopcart-item_right
                height 48px
                display flex
                justify-content flex-start
                align-items center
                .shopcart-item_rightText
                    font-size 14px
                    font-weight bold
                    color rgb(240,20,20)
                    margin-right 12px

  .bottom_box_div
    display: flex;
    justify-content: flex-start;
    .div1 
        width: 56px;
        height: 56px;
        background: red;
        border-radius: 50%;
        margin-top: -10px;
        padding: 6px;
        box-sizing: border-box;
        background: #141d27;
        position relative
        .div1_div 
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background: #2b343c;
            text-align: center;
            color: #fff;
            line-height: 44px;
            font-size: 24px;
            color: #80858a;
            &.activeDiv
                background :rgb(0,160,220)
                color #fff
        .reddain
            position absolute;
            top -5px
            right 0px
            width 30px
            height 16px
            line-height 16px
            font-size 9px
            font-weight bold
            text-align center
            border-radius 12px
            background rgb(240,20,20)
            color #fff
    .jine 
        line-height: 48px;
        font-size: 16px;
        color: rgba(255, 255, 255, 0.4);
        margin-left: 5px;
        padding-right: 12px;
        box-sizing: border-box;
    .psf 
        font-size: 12px;
        color: rgba(255, 255, 255, 0.4);
        font-weight: bold;
        line-height: 48px;
        margin-left: 12px;
  .jiesuan 
    width: 105px;
    height: 48px;
    font-size: 16px;
    color: rgba(255, 255, 255, 0.4);
    line-height: 48px;
    font-weight: bold;
    background:  #2b343c;
    text-align: center;3
    &.activeJiesuan
        background red
.ball-container
    .ball
        position :fixed
        left 32px
        bottom 22px
        z-index 200
        transition: all .4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
        .inner
            width 16px
            height 16px
            transition: all .4s linear
            border-radius 50%
            background: rgb(0,160,220)

        
</style>

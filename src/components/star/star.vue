<template>
    <div class="star_box" :class="typeClass">
       <span v-for="(item,index) in itemClass" :class="item" class="item_class"></span>
    </div>
</template>
<script type='text/ecmascript-6'>
let LENGTH = 5
let ON = 'on'
let HALF = 'half'
let OFF = 'off'
export default {
    props:{
        size:{
            type:Number
        },
        count:{
            type:Number
        }
    },
    computed:{
        typeClass(){
            return `star_${this.size}` 
        },
        itemClass(){
            let result = []
            let val1 = Math.floor(this.count * 2) /2
            let val2 =  Math.floor(this.count)
            for(let i =0;i<val2;i++){
                result.push(ON)
            }
            if(val1 % 1 !== 0){
                result.push(HALF)
            }
            while(result.length < LENGTH){
                result.push(OFF)
            }
            return result
        }
    }
}
</script>
<style scoped lang='stylus' rel='stylesheet/stylus'>
    @import "common/stylus/mixin.styl"
    .star_box
        .item_class
            display:inline-block
            background-repeat: no-repeat
        &.star_48
            .item_class
                width:20px
                height:20px
                background-size:20px 20px
                margin-right:22px
                &:last-child
                    margin-right:0
                &.on
                    bg-image('star48_on')
                &.off
                    bg-image('star48_off')
                &.half
                    bg-image('star48_half')
        
        &.star_48
            .item_class
                width:20px
                height:20px
                background-size:20px 20px
                margin-right:22px
                &:last-child
                    margin-right:0
                &.on
                    bg-image('star48_on')
                &.off
                    bg-image('star48_off')
                &.half
                    bg-image('star48_half')

        &.star_36
            .item_class
                width:15px
                height:15px
                background-size:15px 15px
                margin-right:6px
                &:last-child
                    margin-right:0
                &.on
                    bg-image('star36_on')
                &.off
                    bg-image('star36_off')
                &.half
                    bg-image('star36_half')

        &.star_24
            .item_class
                width:10px
                height:10px
                background-size:10px 10px
                margin-right:3px
                &:last-child
                    margin-right:0
                &.on
                    bg-image('star24_on')
                &.off
                    bg-image('star24_off')
                &.half
                    bg-image('star24_half')

</style>

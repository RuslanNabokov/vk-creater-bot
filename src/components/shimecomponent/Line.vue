<template>
    <div class="line" style="position:absolute;  z-index:9999999" v-bind:style="{height:get_height,left:get_left,top:get_top,width:get_width,backgroundColor:color,transform:'rotate(' +  rotate + ')'}"> </div>
</template>

<script>
export default {
     name:'MLine',
     props:{

         'top': {} ,
         'left': {} ,
         'color':{type:String,default:'green'},

          'width':{default: '5px'},
          'height':{default: '1px'},
          'rotate':{type:String,default:'0 deg'}
         },

    computed:{
        get_top(){ return parseInt(this.top) + 'px'},
        get_left(){  return parseInt(this.left) + 'px'},
        get_height(){return parseInt(this.height) + 'px'},  
        get_width(){return parseInt(this.width) + 'px'}
    }
}
</script>
<style scoped>

</style>
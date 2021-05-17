<template>
    <div class="line" style="position:absolute" v-bind:style="{height:height,left:left,top:top,width:width,backgroundColor:color}"> </div>
</template>

<script>
export default {
     name:'Linetoblock',
     props:{

         'top': String ,
         'left':String,
         'color':{type:String,default:'green'},

          'width':{type: String, default: '5px'},
          'height':{type: String, default: '1px'},
          'rotate':{type:String,default:'0px'}
         },
}
</script>
<style scoped>

</style>
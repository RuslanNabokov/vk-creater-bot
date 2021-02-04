<template>
    <div class="line" style="position:absolute" v-bind:style="{height:height,left:left,top:top,width:width,backgroundColor:color}"> </div>
</template>

<script>
export default {
     name:'Linetoblock',
     props:{

         'top': String ,
         'left':String,
         'color':{type:String,default:'red'},

          'width':{type: String, default: '5px'},
          'height':{type: String, default: '1px'}
         },
}
</script>
<style scoped>

</style>
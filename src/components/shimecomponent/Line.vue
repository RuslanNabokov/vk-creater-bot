<template>
    <div class="line" style="position:absolute; background-color:red" v-bind:style="{height:height,left:left,top:top,width:width}"> </div>
</template>

<script>
export default {
     name:'Linetoblock',
     props:{
         'height': {type: String, default: '5px'} ,
         'top': String  ,
         'left':String ,
         'width':{type: String, default: '5px'},
         }
}
</script>
<style scoped>

</style>
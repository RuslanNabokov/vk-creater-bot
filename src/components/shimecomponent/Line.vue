<template>
    <div style="position:absolute;width:5px; background-color:red" v-bind:style="{height:height,left:left,top:top}"> </div>
</template>

<script>
export default {
     name:'Linetoblock',
     props:['height','top','left']
}
</script>
<style scoped>

</style>
<template>
    <div    style="position:absolute;height:100%;width:100%" >
        <div  v-on:mousemove="mouseMove"   class='field'>
       
        <!-- <div v-for="block in objects" :key="block.id" class='drag'
         v-bind:style="{
        position: positionByKey(block.id),
        left:block.options.left,
        top:block.options.top,
        backgroundColor:block.options.color,
         }"
         v-on:mousedown="myDrag($event,block.id)"
         v-on:mouseup="myDragStop()"
        >
        </div> -->
        <Block
         v-bind:card={id:1,left:left,top:top}

        
         />
        </div>
    </div>

    
</template>

<script>
import Block from  '@/components/shimecomponent/Block'
export default {
    name:'ShimeRedactor',
    data:() => ({
    dragged:-1,
    left:1,
    top:1,
    }),
    components:{Block},
    mounted(){

    },
    methods:{
        mouseMove(event){
            //console.log(event.pageX)
 
            
        },
        myDrag(event,id){
            this.dragged=id;
           
        },
        moveAt(event){

            

        },
    mouseMove(event){
            this.left =     event.pageX - 20 + 'px' ;
            this.top  =  event.pageY  - 110  + 'px';
            console.log(event.pageX )
            
         }
           
    //     },
    //      myDragStop() {
    //     // сбрасываем все значения к начальным
    //     this.dragged= -1;

    //     console.log('drag')
    //     },
    //     isDragging() {
    //     return this.dragged != -1;
    // },
    //     positionByKey(key) {
    //         return this.dragged == key ? 'absolute' : 'relative'
    //     },
    //     leftByKey() {
    //         return this.isDragging() ? this.dragLeft + 'px' : 0
    //     },
    //     topByKey() {
    //         return this.isDragging() ? this.dragTop + 'px' : 0
    //     },


    }
}
</script>

<style  scoped>
    .drag{
        position: relative;
        top:20%;
        left: 10%;
        width:100px;
        height:100px;
        background-color:rgba(0,0,0,0.6);
    }

    .field{
        position: relative;
        width:100%;
        height: 100%;
    }
</style>
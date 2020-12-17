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
        <Block v-for="card in cards"  :key="card.id"
         v-bind:card={...card}
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
    cards:[
        {id:1,color:'red',top:10,left:10,name:"block"},
        {id:2,color:'grey',top:50,left:20,name:"block2"},
        {id:3,color:'yellow',top:100,left:30,name:"block3"},

    ]
    }),
    components:{Block},
    mounted(){
        this.$on('drag', function (id,x,y) { 
            this.dragged = id
            this.clickX = x,
            this.clickY = y
         }), 
        this.$on('stopdrag', function (id) { this.dragged = -1 }) 
    },
    computed:{
   
    },
    methods:{
    get_position(id){
            let searh_coord = document.getElementById(id+'')
            let x = searh_coord.getBoundingClientRect().x
            let y = searh_coord.getBoundingClientRect().y 
            return {x:x,y:y}
        },
        myDrag(id){
            this.dragged=2;
         
        },
        moveAt(event){           

        },
    mouseMove(event){
            if(this.dragged == -1){ return}
            let active = this.cards[this.cards.findIndex( card => card.id === this.dragged ) ]
            active.left =  event.clientX - this.clickX  +  'px' ;
            active.top  =  event.clientY - this.clickY  + document.documentElement.scrollTop   - 80    + 'px';
            let na_odn_osi_x = this.cards.find( card => card.id !== this.dragged && ((parseInt(this.get_position(card.id).x ) -  parseInt(active.left) > 2    )  ||(parseInt(this.get_position(card.id).x ) -  parseInt(active.left) > -2   )   )    )
            let na_odn_osi_y = this.cards.find( card => card.id !== this.dragged && ((parseInt(this.get_position(card.id).y ) -  parseInt(active.left) > 2    )  ||(parseInt(this.get_position(card.id).y ) -  parseInt(active.left) > -2   )   )    )
            if (na_odn_osi_x.length != 0){
                if (active.top >  parseInt(na_odn_osi_x.top)){

                }
            }
        },
           

    myDragStop(id) {
        if (this.dragged == id){
       // this.dragged= -1
        console.log('s')
        }else{}
        },
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
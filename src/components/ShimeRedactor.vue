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
         <component v-bind:is="linecurrent" 
         v-bind:height="lineHeight" 
         v-bind:top="lineTop"
         v-bind:left="lineLeft"
         ></component>
        </div>
    </div>

    
</template>

<script>



import Block from  '@/components/shimecomponent/Block'
import Linetoblock from  '@/components/shimecomponent/Line'
export default {
    name:'ShimeRedactor',
    data:() => ({
    dragged:-1,
    left:1,
    top:1,
    lineHeight:"200px",
    lineTop:"200px",
    lineLeft:"200px",
    linecurrent:'Linetoblock',
    cards:[
        {id:1,color:'red',top:10,left:10,name:"block"},
        {id:2,color:'grey',top:50,left:20,name:"block2"},
        {id:3,color:'yellow',top:100,left:30,name:"block3"},

    ]
    }),
    components:{Block,Linetoblock},
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
    get_size(id){
         let searh_coord = document.getElementById(id+'')
          let width = searh_coord.getBoundingClientRect().width
        let height = searh_coord.getBoundingClientRect().height
        return {width:width,height:height}
    },
        myDrag(){
            
         
        },
        moveAt(event){           

        },
    mouseMove(event){
            if(this.dragged == -1){ return}
            let active = this.cards[this.cards.findIndex( card => card.id === this.dragged ) ]
            active.left =  parseInt(event.clientX) - parseInt(this.clickX)  - 10   +  'px' ;
            active.top  =  parseInt(event.clientY) - parseInt(this.clickY)  + parseInt(document.documentElement.scrollTop)  - 70    + 'px';
           
            let na_odn_osi_x = this.cards.find( card => card.id !== this.dragged && (
            (parseInt(this.get_position(card.id).x ) -  parseInt(active.left) > -5 && parseInt(this.get_position(card.id).x ) -  parseInt(active.left) < 5)  
            ||
            (parseInt(this.get_position(card.id).x ) -  parseInt(active.left) < -5 && parseInt(this.get_position(card.id).x ) -  parseInt(active.left) > 5)  )    )
            let na_odn_osi_y = this.cards.find( card => card.id !== this.dragged &&
            (parseInt(this.get_position(card.id).y ) -  parseInt(active.top) > -5 && parseInt(this.get_position(card.id).y ) -  parseInt(active.top) < 5)  
            ||
            (parseInt(this.get_position(card.id).y ) -  parseInt(active.top) < 5 && parseInt(this.get_position(card.id).y ) -  parseInt(active.top) > -5)  )   
           
            if (na_odn_osi_x &&   na_odn_osi_x.length != 0){
                
                if (parseInt(active.top) >  parseInt( this.get_position(na_odn_osi_x.id).y)){
                    
                       this.lineLeft = parseInt(active.left) + 60 + 'px'
                       this.lineTop =parseInt(this.get_position(na_odn_osi_x.id).y) + 'px'
                       
                       this.lineHeight = ( parseInt(active.top) - parseInt(this.get_position(na_odn_osi_x.id).y) ) + (this.get_size(active.id).height / 2  + this.get_size(na_odn_osi_x.id).height / 2   )  + 'px'
                       this.linecurrent = 'Linetoblock'
                       } 
            }
        },
           

    myDragStop(id) {
        if (this.dragged == id){
       // this.dragged= -1
        
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
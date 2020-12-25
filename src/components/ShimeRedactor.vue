<template>
    <div    style="position:absolute;height:100%;width:100%" >
        <div style="overflow: hidden;"  v-on:mousemove="mouseMove"   class='field'>
       
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
         <component v-bind:is="linecurrentX" 
         v-bind:height="lineHeightX" 
         v-bind:top="lineTopX"
         v-bind:left="lineLeftX"
         width:='15px'
         ></component>
        <component v-bind:is="linecurrentY" 
         v-bind:top="lineTopY"
         v-bind:left="lineLeftY"
         v-bind:width="lineWidthY"
         ></component>

                 <component v-bind:is="linecurrentY2" 
         v-bind:top="lineTopY2"
         v-bind:left="lineLeftY2"
         v-bind:width="lineWidthY2"
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
    resize:-1,
    left:1,
    top:1,
    lineHeightX:"250px",
    lineTopX:"200px",
    lineLeftX:"200px",
    lineWidthY:"200px",
    lineTopY:"200px",
    lineLeftY:"200px",
    lineWidthY2:"200px",
    lineTopY2:"200px",
    lineLeftY2:"200px",

    linecurrentX:'Linetoblock',
    linecurrentY:'Linetoblock',
    linecurrentY2:'Linetoblock',
  
    cards:[
        {id:1,color:'red',top:10,left:10,name:"block",width:'200px',height:'200px'},
        {id:2,color:'grey',top:50,left:20,name:"block2",width:'200px',height:'200px'},
        {id:3,color:'yellow',top:100,left:30,name:"block3",width:'200px',height:'200px'},

    ]
    }),
    components:{Block,Linetoblock},
    mounted(){
        this.$on('drag', function (id,x,y) { 
            this.dragged = id
            this.clickX = x,
            this.clickY = y
            
         }), 
        this.$on('resize',function(id,x,y){
 
        this.resize = id;
        

        }), 
        


        this.$on('stopdrag', function (id) { 
        this.resize = -1;
        this.dragged = -1;
        this.linecurrentX = '';
        this.linecurrentY = '';
        this.linecurrentY2 = '';
        })
    },
    computed:{
   
    },
    methods:{
    get_position(id){
            let searh_coord = document.getElementById(id+'')
            let x = searh_coord.getBoundingClientRect().x
            let y = searh_coord.getBoundingClientRect().y
            return {x:x,y:y,left:x,top:y}
        },
    distance_x_check(block1,block2,dist=5){  //для одной по x
    let dist_left = dist
     dist_left =    Math.abs(  this.get_position(block1.id).y  -  this.get_position(block2.id).y)
    return  parseInt(dist_left) <= parseInt(dist)

    },
    distance_x_check_center(block1,block2,dist=1){ //для одной по x
    let dist_left = dist
     dist_left  =    Math.abs(  (this.get_position(block1.id).y  + this.get_size(block1.id).width / 2   )     - (  this.get_position(block2.id).y +  this.get_size(block2.id).width / 2     ))
   
    return  parseInt( dist_left ) <= dist
    },
    distance_x_check_bott(block1,block2,dist=1){
         let dist_left  =    Math.abs(  (this.get_position(block1.id).y  + this.get_size(block1.id).height    )     - (  this.get_position(block2.id).y +  this.get_size(block2.id).height     ))
        return   parseInt( dist_left ) <= dist
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
            if(this.resize != -1){ 
                let active = this.cards[this.cards.findIndex( card => card.id === this.resize ) ]
                active.width =    parseInt(event.clientX)  - this.get_position(active.id).x + 1    + 'px' 
                 active.height =  parseInt(event.clientY)  - this.get_position(active.id).y + 1    + 'px' 
                console.log( event)

            }
            if(this.dragged == -1){ return}
            let active = this.cards[this.cards.findIndex( card => card.id === this.dragged ) ]
            active.left =  parseInt(event.clientX) - parseInt(this.clickX)  - 10   +  'px' ;
            active.top  =  parseInt(event.clientY) - parseInt(this.clickY)  + parseInt(document.documentElement.scrollTop)  - 70    + 'px';
            let na_odn_osi_y = this.cards.filter( card => card.id !== this.dragged && (
            (parseInt(this.get_position(card.id).x    ) -  parseInt(active.left) > -1 && parseInt(this.get_position(card.id).x ) -  parseInt(active.left) < 1)  
            ||
            (parseInt(this.get_position(card.id).x ) -  parseInt(active.left) < -1 && parseInt(this.get_position(card.id).x ) -  parseInt(active.left) > 1)  )    )
           let na_odn_osi_x = this.cards.filter( card => card.id !== this.dragged &&  this.distance_x_check(active,card,0.2) )
           let na_odn_osi_x_center = this.cards.filter( card => card.id !== this.dragged &&  this.distance_x_check_center(active,card,0.2) )
           let na_odn_osi_x_bott =  this.cards.filter( card => card.id !== this.dragged &&  this.distance_x_check_bott(active,card,0.2) )
            if (na_odn_osi_x_bott.length){
                     let all_array = na_odn_osi_x.slice()
               all_array.push(active)
               let sort_y =  all_array.sort((a,b)=>{  if (parseInt(a.left) <  parseInt(b.left) ){return -1}else if (parseInt(a.left) >  parseInt(b.left)) {return 1 }else{return 0}     })
                this.lineTopY =    parseInt(active.top) + 7 + 'px'
                this.lineLeftY = parseInt(sort_y[0].left) +  parseInt(sort_y[0].width)  + 'px'    
                this.lineWidthY =   parseInt(this.get_position(sort_y.slice(-1)[0].id).x)  -   parseInt(this.get_position(sort_y[0].id).x)   -   parseInt(sort_y.slice(-1)[0].width)      + 'px'
                this.linecurrentY = 'Linetoblock'
            }else{

            }
            
            if (na_odn_osi_y.length){
                let all_array = na_odn_osi_y.slice()
                all_array.push(active)
                let sort_x =  all_array.sort((a,b)=>{  if (parseInt(a.top) <  parseInt(b.top) ){return -1}else if (parseInt(a.top) >  parseInt(b.top)) {return 1 }else{return 0}     })
                this.lineTopX =  parseInt(sort_x[0].top) +   parseInt( this.get_size(sort_x[0].id).height /2 )  + 'px'     
                this.lineLeftX = parseInt(active.left) +  parseInt( this.get_size(sort_x[0].id).width /2 )  + 'px' 
             //   this.lineHeightX =   ( parseInt(na_odn_osi_x.slice(-1)[0].top) - parseInt(parseInt(na_odn_osi_x[0].top) ) ) + (parseInt(this.get_size( na_odn_osi_x.slice(-1)[0].id  ).height / 2 ) + parseInt(this.get_size(na_odn_osi_x[0].id).height / 2 )  )  + 'px'
                this.lineHeightX =   parseInt(this.get_position(sort_x.slice(-1)[0].id).y)  -   parseInt(this.get_position(sort_x[0].id).y)       + 'px'
                
                // let position)
                // let os_start_x = parseInt(active.top) >  parseInt( this.get_position(na_odn_osi_x.id).y)   ?  parseInt(active.left) + 60 + 'px'
                // if (){
                    
                //        this.lineLeft = parseInt(active.left) + 60 + 'px'
                //        this.lineTop =parseInt(this.get_position(na_odn_osi_x.id).y) + 'px'
                       
                //        this.lineHeight = ( parseInt(active.top) - parseInt(this.get_position(na_odn_osi_x.id).y) ) + (this.get_size(active.id).height / 2  + this.get_size(na_odn_osi_x.id).height / 2   )  + 'px'
                //        this.linecurrent = 'Linetoblock'
                //        }
                this.linecurrentX = 'Linetoblock'
            }else{
                this.linecurrentX = ''
            }
            if (na_odn_osi_x.length){
            let all_array = na_odn_osi_x.slice()
               all_array.push(active)
               let sort_y =  all_array.sort((a,b)=>{  if (parseInt(a.left) <  parseInt(b.left) ){return -1}else if (parseInt(a.left) >  parseInt(b.left)) {return 1 }else{return 0}     })
                this.lineTopY =    parseInt(active.top) + 7 + 'px'
                this.lineLeftY = parseInt(sort_y[0].left) +  parseInt(sort_y[0].width)  + 'px'    
                this.lineWidthY =   parseInt(this.get_position(sort_y.slice(-1)[0].id).x)  -   parseInt(this.get_position(sort_y[0].id).x)   -   parseInt(sort_y.slice(-1)[0].width)      + 'px'
                this.linecurrentY = 'Linetoblock'
           }else{
               this.linecurrentY = ''
           }
           if (na_odn_osi_x_center.length){
             
             let all_array =na_odn_osi_x_center.slice()
               all_array.push(active)
               let sort_y =  all_array.sort((a,b)=>{  if (parseInt(a.left) <  parseInt(b.left) ){return -1}else if (parseInt(a.left) >  parseInt(b.left)) {return 1 }else{return 0}     })
                this.lineTopY2 =    parseInt(active.top) +  parseInt( this.get_size(active.id).height /2 )  + 'px'
                this.lineLeftY2 = parseInt(sort_y[0].left) +  parseInt( this.get_size(sort_y[0].id).height /2 )  + 'px'  
                this.lineWidthY2 =   parseInt(this.get_position(sort_y.slice(-1)[0].id).x)  -   parseInt(this.get_position(sort_y[0].id).x)       + 'px'
                this.linecurrentY2 = 'Linetoblock'

           }else{
            this.linecurrentY2 = ''
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
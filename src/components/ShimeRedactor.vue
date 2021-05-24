<template>
    <div    style="position:absolute;height:100%;width:100%" >
        <div style="overflow: hidden;"  v-on:mousemove="mouseMove"      class='field'>
       
        <!-- @contextmenu="rightclick($event)"  
            <div v-for="block in objects" :key="block.id" class='drag'
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
        <MainMenu
        v-bind:dedicated=dedicated
        v-bind:cards=cards
         />
        <!-- <ContextMenu/> -->
        <Block v-for="card in cards"  :key="card.id"
         v-bind:card={...card}
         v-bind:dedicated=isdedicated(card.id)
         />
        
        <MLine  v-for="(line,id) in  lines" 
         v-bind:top=line.top
          v-bind:width=line.width
         v-bind:left=line.left
         v-bind:color=line.color
        />

        <Linetoblock v-for="connect in connections" 
        v-bind:x1=cards[connect.card_id].left
        v-bind:y1=cards[connect.card_id].top
        v-bind:x2=cards[connect.to_card_id].left
        v-bind:y2=cards[connect.to_card_id].top
       
        v-bind:current_pos_x=current_pos_x
         v-bind:current_pos_y=current_pos_y
         v-bind:peres=calc_peres_card(connect.card_id,connect.to_card_id)
        > </Linetoblock>

 
 <Linetoblock  v-if = "load" 
            v-bind:x1=get_center_window
            v-bind:x2= get_center(get_main_card.id).x
            y1=0
            v-bind:y2=get_main_card.top
            v-bind:peres=calc_peres(get_center_window,0,get_center(get_main_card.id).x,get_main_card.top)
         />
        </div>

        
    </div>


</template>

<script>



import Block from  '@/components/shimecomponent/Block'
import MLine from  '@/components/shimecomponent/Line'
// import ContextMenu from './context/ContextMenu.vue'
import MainMenu from './context/MainMenu.vue'
import Linetoblock from  '@/components/shimecomponent/LinetoBlock'
import LinetoBlock from './shimecomponent/LinetoBlock.vue'


const AXIS_ERR = 10
export default {
    name:'ShimeRedactor',
    data:() => ({
    window_width: window.innerWidth,
    load:false,
    dragged:-1,
    dedicated:[],
    resize:-1,
    left:1,
    top:1,
    lines:[],
    current_pos_x:0,
    current_pos_y:0,
    connections: new Array({card_id:1,to_card_id:2,type:'common_line',height:200 }),

  
    cards:[
        {id:1,color:'red',top:10,left:10,name:"block",width:'200px',height:'200px',overlap_opacity:false,type:'main'} ,
        {id:2,color:'grey',top:50,left:20,name:"block2",width:'200px',height:'200px',overlap_opacity:false},
        {id:3,color:'yellow',top:100,left:30,name:"block3",width:'200px',height:'200px',overlap_opacity:false},

    ]
    }),
    components:{Block,
                MLine,
                Linetoblock,
                MainMenu,
        LinetoBlock},
    mounted(){

        this.load = true

        this.$on('addBlockInarraydedicated',(card_id)=>{
            let index_ = this.dedicated.indexOf(card_id)
            if (index_ == -1){
            this.dedicated.push(card_id)}else{ 
                this.dedicated.splice(index_,1)
            }

         }),

        this.$on('drag', function (id,x,y) { 
            this.dragged = id
            this.clickX = x,
            this.clickY = y
          
            document.getElementById(`block-${id}`).style.zIndex = "9999"
         }), 
        this.$on('resize',function(id,x,y){
        this.resize = id;
        // document.getElementById(`block-${id}`).style.zIndex = "9999"
        // let active = this.cards[this.cards.findIndex( card => card.id === this.resize ) ]
        }), 
        


        this.$on('stopdrag', function (id) { 
        let active_ = this.cards[this.cards.findIndex( card => card.id === this.resize || card.id === this.dragged  ) ]
        if (active_){
        active_.overlap_opacity = false
        }
        
       this.resize = -1;
        this.dragged = -1;
         this.lines = []
        
    document.getElementById(`block-${id}`).style.zIndex = "10"
         try {
           this.cards.forEach( function (e){
                        e.info_bot = ''
                        e.info_left = ''                    }
                 ) 
         } catch {

         } 
            
        

        })
    },
    computed:{
        get_center_window(){
            return this.window_width /2
        },

    get_main_card(){

       let main_card = this.cards.filter((el)=>{ 
         return   el.type == 'main'
       })

       return main_card[0]
    }, 

    },  
     watch:{

     },
     created(){
          window.addEventListener('resize', this.updateWidth);
     },


    
    methods:{
    
 

      updateWidth() {
        this.window_width = window.innerWidth;
    },
    

    rightclick(event){ 
        console.log(event)
         event.preventDefault();
    },

    showContextMenu(){

    },

    isdedicated(id){
        return  this.dedicated.indexOf(id) != -1
    },


    calc_peres(x1,y1,x2,y2){
        let k = (parseFloat(y1) - parseFloat(y2))  / (parseFloat(x1) - parseFloat(x2) );
        let b =   parseFloat(y2)  - (parseFloat(k) * parseFloat(x2));
        let pred_y =  Math.abs(parseFloat(k) * parseFloat(this.current_pos_x) +  parseFloat(b));  
        return   (pred_y + 53) - Math.abs(parseFloat(this.current_pos_y))  >= -10 && (pred_y + 53) - Math.abs(parseFloat(this.current_pos_y))  <= 10
    },
    calc_peres_card(card_id,to_card_id ){
        let x1=this.cards[card_id].left
        let y1= this.cards[card_id].top
        let x2= this.cards[to_card_id].left
        let y2= this.cards[to_card_id].top
        return this.calc_peres(x1,y1,x2,y2)
    },


    get_position(id){
   
            let searh_coord = document.getElementById(`block-${id}`)
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
         let searh_coord = document.getElementById(`block-${id}`)
          let width = searh_coord.getBoundingClientRect().width
        let height = searh_coord.getBoundingClientRect().height
        return {width:width,height:height}
    },
    get_center(id){

    let x =  parseInt(this.get_position(id).x)  + parseInt(this.get_size(id).width) /2
    let y =   parseInt(this.get_position(id).y)  + parseInt(this.get_size(id).height ) /2
    return {x,y}
    },

     get_bottom(id){
       let x =  parseInt(this.get_position(id).x)  
    let y =   parseInt(this.get_position(id).y)  + parseInt(this.get_size(id).height ) 
   
    return {x,y}
    },

        myDrag(){
            
         
        },

    mouseMove(event){
               // this.dragged= -1
            // current_pos_y
//             offsetX: 865
// offsetY: 485
// pageX: 865
// pageY: 548
            this.current_pos_x = event.clientX
            this.current_pos_y = event.clientY
            if(this.resize != -1){ 
                let active = this.cards[this.cards.findIndex( card => card.id === this.resize ) ]
                this.cards.forEach( function (e){
                        e.info_bot = ''
                        e.info_left = ''                    }
                 ) 
                active.width =    parseInt(event.clientX)  - this.get_position(active.id).x + 1    + 'px' 
                active.height =  parseInt(event.clientY)  - this.get_position(active.id).y + 10    + 'px'
                    
                active.info_bot = parseInt(active.width)
                active.info_left = parseInt(active.height)

                let odn_razm_width  = this.cards.filter( card => card.id !== this.resize && ( 
                        parseInt(card.width) == parseInt(active.width)   
                        ) )
                 let odn_razm_height  = this.cards.filter( card => card.id !== this.resize && ( 
                        parseInt(card.height) == parseInt(active.height)   
                        ) )

                if (this.dedicated.indexOf(this.resize) != -1){
                 
                    this.dedicated.forEach(element => {
                        let card =  this.cards[this.cards.findIndex( card => card.id == element ) ]

                        card.width = active.width
                        card.height = active.height
                        card.info_bot = active.info_bot
                        card.info_left = active.info_left

                    });
                }
                odn_razm_width.forEach(e => e.info_bot =  parseInt(e.width) );
                odn_razm_height.forEach(e => e.info_left =  parseInt(e.height) );
                
            }
            if(this.dragged == -1 && this.resize == -1  ){ return}
            this.lines=[]
            let active = this.cards[this.cards.findIndex( card => card.id == this.resize  || card.id == this.dragged     ) ]
            
            if (this.resize == -1 ){
           
            active.left =  parseInt(event.clientX) - parseInt(this.clickX)    +  'px' ;
            active.top  =  parseInt(event.clientY) - parseInt(this.clickY)  + parseInt(document.documentElement.scrollTop) - 64 + 'px';
            let overlab_block = new Array()
            overlab_block  =   this.cards.filter(card => card.id  !== this.dragged &&  card.id !== this.resize &&

            (
            (
            (parseInt(this.get_position(active.id).y) + parseInt(this.get_size(active.id).height)   -   
             parseInt(this.get_position(card.id).y)   <=  parseInt(this.get_size(card.id).height) 
            && 
            parseInt(this.get_position(active.id).y) + parseInt(this.get_size(active.id).height)   -   
            parseInt(this.get_position(card.id).y)     > 0 )
            ||
            (
            parseInt(this.get_position(card.id).y) + parseInt(this.get_size(card.id).height)   -   
            parseInt(this.get_position(active.id).y)   <= parseInt(this.get_size(active.id).height) 
             &&
             parseInt(this.get_position(card.id).y) + parseInt(this.get_size(card.id).height)  -
            parseInt(this.get_position(active.id).y)     > 0 
            )
            )
            &&(
            (parseInt(this.get_position(active.id).x) + parseInt(this.get_size(active.id).width)   -
             parseInt(this.get_position(card.id).x)   <=  parseInt(this.get_size(card.id).width)
            &&
            parseInt(this.get_position(active.id).x) + parseInt(this.get_size(active.id).width)   -
            parseInt(this.get_position(card.id).x)     > 0 )
            ||
            (
            parseInt(this.get_position(card.id).x) + parseInt(this.get_size(card.id).width)   -
            parseInt(this.get_position(active.id).x)   <= parseInt(this.get_size(active.id).width)
             &&
             parseInt(this.get_position(card.id).x) + parseInt(this.get_size(card.id).width)  -
            parseInt(this.get_position(active.id).x)     > 0
            )
            )
            )
            )
           if(overlab_block.length){
             active.overlap_opacity  = true
           }else{
               active.overlap_opacity=false
           }
            }
            let na_ond_osi_x_top =  this.cards.filter( card => card.id !== this.dragged  && card.id !== this.resize  &&  (  
                Math.abs( Math.abs(parseInt(this.get_position(active.id).y)  - parseInt(this.get_position(card.id).y)))   <=  AXIS_ERR 
                || 
        Math.abs(  Math.abs(parseInt(this.get_position(active.id).y )  - parseInt(this.get_center(card.id).y)))   <=  AXIS_ERR
            ||
            Math.abs( Math.abs(parseInt(this.get_position(active.id).y )  - parseInt(this.get_bottom(card.id).y)) )  <=  AXIS_ERR
                ) )
            let na_odn_osi_x_center = this.cards.filter( card => card.id !== this.dragged  && card.id !== this.resize && (  
        Math.abs(  Math.abs(parseInt(this.get_center(active.id).y)  - parseInt(this.get_position(card.id).y)) )  <=  AXIS_ERR
            || 
        Math.abs(  Math.abs(parseInt(this.get_center(active.id).y )  - parseInt(this.get_center(card.id).y))  ) <=  AXIS_ERR
            ||
        Math.abs(   Math.abs(parseInt(this.get_center(active.id).y )  - parseInt(this.get_bottom(card.id).y)) )  <=  AXIS_ERR
                ) )
            let na_odn_osi_x_bottom = this.cards.filter( card =>  card.id !== this.dragged  && card.id !== this.resize && (  
        Math.abs(  Math.abs(parseInt(this.get_bottom(active.id).y)  - parseInt(this.get_position(card.id).y)) )  <=  AXIS_ERR
            || 
        Math.abs(  Math.abs(parseInt(this.get_bottom(active.id).y )  - parseInt(this.get_center(card.id).y))  )  <= AXIS_ERR
            ||
        Math.abs(   Math.abs(parseInt(this.get_bottom(active.id).y )  - parseInt(this.get_bottom(card.id).y)) )  <=  AXIS_ERR
          
                ) )
 
     
            if ( na_ond_osi_x_top.length){
              let all_array = na_ond_osi_x_top.slice()
               all_array.push(active)
               let sort_y =  all_array.sort((a,b)=>{  if (parseInt(a.left) <  parseInt(b.left) ){return -1}else if (parseInt(a.left) >  parseInt(b.left)) {return 1 }else{return 0}     })
                let top_line  =    parseInt(active.top)  + 'px'
                 let left_line = parseInt(sort_y[0].left) +  parseInt(sort_y[0].width)  + 'px'    
                let width_line  =   parseInt(this.get_position(sort_y.slice(-1)[0].id).x)   -  parseInt(sort_y[0].width) -   parseInt(this.get_position(sort_y[0].id).x)           + 'px'
                this.lines.push({width:width_line, top:top_line,  left:left_line})
                console.log(event)
                console.log(active.left + ' active left')
                if (   Math.abs(Math.abs(event.pageY) - ( Math.abs( parseInt(active.left) + parseInt(this.clickX ) )    ))<= 50  ) {

                        active.top = sort_y[0].top
                }else{

                }
               
               
            }else{}
           if  (na_odn_osi_x_center.length){
                 let all_array = na_odn_osi_x_center.slice()
               all_array.push(active)
               let sort_y =  all_array.sort((a,b)=>{  if (parseInt(a.left) <  parseInt(b.left) ){return -1}else if (parseInt(a.left) >  parseInt(b.left)) {return 1 }else{return 0}     })
                let top_line  =    parseInt(active.top) +  ( parseInt(this.get_size(active.id).height) / 2  )  + 'px'
                 let left_line = parseInt(sort_y[0].left) +   ( parseInt(this.get_size(sort_y[0].id).width) / 2  )   + 'px'    
                let width_line  =   parseInt(this.get_position(sort_y.slice(-1)[0].id).x)   +  ( parseInt(this.get_size(sort_y.slice(-1)[0].id).width) / 2  )  -   parseInt(this.get_position(sort_y[0].id).x)      -  ( parseInt(this.get_size(sort_y[0].id).width) / 2  )      + 'px'
                
               
                this.lines.push({width:width_line, top:top_line,  left:left_line})
           }else{}
              if  (na_odn_osi_x_bottom.length){
                let all_array = na_odn_osi_x_bottom.slice()
               all_array.push(active)
             
               let sort_y =  all_array.sort((a,b)=>{  if (parseInt(a.left) <  parseInt(b.left) ){return -1}else if (parseInt(a.left) >  parseInt(b.left)) {return 1 }else{return 0}     })
                let top_line  =    parseInt(active.top)  +  parseInt(active.height)  + 'px'
                 let left_line = parseInt(sort_y[0].left) +  parseInt(sort_y[0].width)  + 'px'    
                let width_line  =   parseInt(this.get_position(sort_y.slice(-1)[0].id).x)   -  parseInt(sort_y[0].width) -   parseInt(this.get_position(sort_y[0].id).x)           + 'px'
                this.lines.push({width:width_line, top:top_line,  left:left_line})
           }else{}
          
        //     let na_odn_osi_y = this.cards.filter( card => card.id !== this.dragged && (
        //     (parseInt(this.get_position(card.id).x    ) -  parseInt(active.left) > -1 && parseInt(this.get_position(card.id).x ) -  parseInt(active.left) < 1)  
        //     ||
        //     (parseInt(this.get_position(card.id).x ) -  parseInt(active.left) < -1 && parseInt(this.get_position(card.id).x ) -  parseInt(active.left) > 1)  )    )
        //    let na_odn_osi_x = this.cards.filter( card => card.id !== this.dragged &&  this.distance_x_check(active,card,0.2) )
        //    let na_odn_osi_x_center = this.cards.filter( card => card.id !== this.dragged &&  this.distance_x_check_center(active,card,0.2) )
        //    let na_odn_osi_x_bott =  this.cards.filter( card => card.id !== this.dragged &&  this.distance_x_check_bott(active,card,0.2) )
           
  
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
    .card{
        margin:0px  !important;
    }
</style>
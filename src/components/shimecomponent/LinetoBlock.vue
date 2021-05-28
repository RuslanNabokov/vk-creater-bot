<template>
<div class='root' >
    <svg   v-if="type=='common'"  ref='chart'>
  <line  class='line' v-bind:x1="get_x1" v-bind:y1="get_y1" v-bind:x2="get_x2" v-bind:y2="get_y2" :style="{ 'stroke-width': peres ? '10' : '2' }"  style="stroke:rgb(255,255,0);stroke-width:2" />

 </svg>

  <svg   v-else  ref='chart'>
 <line   class='line_1' v-bind:x1="line_1.x" v-bind:y1="line_1.y" v-bind:x2="line_1.x2" v-bind:y2="line_1.y2" :style="{ 'stroke-width': peres ? '10' : '2' }"  style="stroke:rgb(255,255,0);stroke-width:2" />
           <line   class='line_2' v-bind:x1="line_2.x" v-bind:y1="line_2.y" v-bind:x2="line_2.x2" v-bind:y2="line_2.y2" :style="{ 'stroke-width': peres ? '10' : '2' }"  style="stroke:rgb(255,255,0);stroke-width:2" />

  </svg>
</div>
</template>

<script>
// import Victor from 'victor'
export default {
     name:'Linetoblock',
     data:()=>({
         k: 0,
     }),
     props:{
         'type':{default:'common'},
         'peres':{default:0},
        'current_pos_x': {default:0},
        'current_pos_y':{default: 0},
         'size_main_block':{default:0},
          'size_second_block':{default:0},  
         'x1': {default:10} ,
         'y1': {default:10} ,
         'x2':{default:100},
         'y2':{default:100},
         'color':{type:String,default:'green'},
          'width':{default: '5px'},
          'height':{default: '5px'},
         },
    mounted(){
    //     this.svg =  d3.select(this.$refs.chart)
    //     let vec_1 = new Victor(parseInt(this.x1),parseInt(this.y1));
    //     let vec_2 =  new Victor(parseInt(this.x2),parseInt(this.y2));
    //     this.vect_arr = []
    //    for (const x    of Array(500).keys()) {
    //             let vec = vec_1.mix(vec_2,0.9)

    //             this.vect_arr.push([vec.x,vec.y] )
                
    //     }
    },
    
    computed:{
        get_x1(){ return parseInt(this.x1) + 'px'},
        get_y1(){  return parseInt(this.y1) + 'px'},
        get_x2(){return parseInt(this.x2) + 'px'},  
        get_y2(){return parseInt(this.y2) + 'px'},
        line_1(){ return  {x: parseInt(parseInt(this.size_main_block.width )  /2) + parseInt(this.x1) 
          , x2: parseInt(this.x1), y:parseInt(this.y1), y2:parseInt(this.y2) }  },
        line_2(){  return  {x:parseInt(this.x1), x2: parseInt(this.x2), y:parseInt(this.y2), y2:parseInt(this.y2)    } }, 

    },
    methods:{
    }
}
</script>
<style scoped>
svg{ 
    position: absolute;
    top:0px;
    left:0px;
    z-index: -1;
    height: 100%;
    width: 100%;
}



.line{ 
     transition: all .5s;
     stroke-width:2px
}
svg>.line:hover{ 
     stroke-width:5px;
     transition: all .5s;
     stroke-width:10;


}

</style>



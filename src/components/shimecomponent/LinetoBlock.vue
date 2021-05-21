<template>
<div class='root' >
    <svg  ref='chart'>
  <line   class='line' v-bind:x1="x1" v-bind:y1="y1" v-bind:x2="x2" v-bind:y2="y2" style="stroke:rgb(255,255,0);stroke-width:2" />
</svg>
</div>
</template>

<script>
import Victor from 'victor'
export default {
     name:'Linetoblock',
     props:{
        'current_pos_x': {default:0},
        'current_pos_y':{default: 0},
         'x1': {default:10} ,
         'y1': {default:10} ,
         'x2':{default:100},
         'y2':{default:100},
         'color':{type:String,default:'green'},
          'width':{default: '5px'},
          'height':{default: '5px'},
         },
    mounted(){
        this.svg =  d3.select(this.$refs.chart)
        let vec_1 = new Victor(parseInt(this.x1),parseInt(this.y1));
        let vec_2 =  new Victor(parseInt(this.x2),parseInt(this.y2));
        this.vect_arr = []
       for (const x    of Array(500).keys()) {
                let vec = vec_1.mix(vec_2,0.9)

                this.vect_arr.push([vec.x,vec.y] )
                
        }
    },
    
    computed:{
        get_x1(){ return parseInt(this.x1) + 'px'},
        get_y1(){  return parseInt(this.y1) + 'px'},
        get_x2(){return parseInt(this.x2) + 'px'},  
        get_y2(){return parseInt(this.y2) + 'px'},
        koef(){ this.k = (parseInt(this.y1) - parseInt(this.y2))  / (parseInt(this.x1) - parseInt(this.x2) );this.b =   parseInt(this.y2)  - (parseInt(this.k) * parseInt(this.x2)); return [this.k,this.b]   },
        peres(){ return     parseInt(this.k) * parseInt(this.current_pos_x) +  parseInt(this.b) -  parseInt(this.current_pos_y)   }
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



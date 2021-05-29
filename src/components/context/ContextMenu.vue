<template>
<div class='context' v-if="viewMenu"   v-bind:style="{left:left_px,top:top_px }">

 <ul id="right-click-menu" tabindex="-1"  >
   <li v-for="(node,index)  in tree">  
      <div  v-bind:class="{ selected: selected(index) }"      @mouseover="MouseOn(index)"  :id=index>
        {{ node.label }}
        </div >
          <ul  class="right-click-menu"     v-if="node.children && node.children.length">
      <Tree  v-bind:select_=select  v-for="(child,index_ch ) in node.children" :node="child" :parent_id=index :id_item=index_ch ></Tree>
    </ul>
     
       </li>

 
    </ul>
</div>
</template>


<script>
import Tree from './TreeContext.vue'


export default {
 props: {
 
        'viewMenu':{default:false},
        'top': {default:'200px'},
        'left':{default: '0px'},
         'ext_menu':{default:'0px'},
        'element_context':{default:-1},

    },
  mounted(){
      this.$on('select_el',(id)=>{
                this.select = id
              
         })
            },
    
  data:() => ({
      tree: [
        {label:'1',children:[{label:'1.1'}],children:[    {label:'2',children:[{label:'2.1'}]}    ]   },
        {label:'2',children:[{label:'2.1'}]}
      
      ],
      select:'0.0' 
    // .push(this.element_context)
  }),
  components:{
    Tree
  },
  methods:{
    MouseOn(index){
          this.select = index 
          
    },
   selected(index){
        
      return   String(this.select).split('.').length < 2 &&  parseInt(this.select) == parseInt(index)  
    },
  },
  computed:{

    left_px(){
      return parseInt(this.left) + 'px'
    },
     top_px(){
      return parseInt(this.top) + 'px'
    },


  }

}
</script>

<style scoped>
  .selected{
  background-color:greenyellow;
}
.context{
  position: absolute;
}
#right-click-menu{
    background: #FAFAFA;
    border: 1px solid #BDBDBD;
    box-shadow: 0 2px 2px 0 rgba(0,0,0,.14),0 3px 1px -2px rgba(0,0,0,.2),0 1px 5px 0 rgba(0,0,0,.12);
    display: block;
    list-style: none;
    margin: 0;
    padding: 0;
   
    width: min-content ;
    z-index: 999999;
    font-size: 10px;

}



</style>
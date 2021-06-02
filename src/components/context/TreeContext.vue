<template>

  <li class='node' > 
    <div v-bind:id="get_id()"  v-bind:class="{ selected: selected }"  v-on:click="action_click(node.index)"  @mouseover.stop="MouseOn()" @mouseleave="mouseleave" >{{ node.label }}</div>

    <ul  class="right-click-menu"     v-if="node.children && node.children.length">
      <node    v-for="(child,index ) in node.children" :node="child"  :select_=select_  :parent_id="get_id()" :id_item="index" ></node>
    </ul>
    </li>
   
</template> 

<script>
export default {
  name: "node",
  hover:1,
  props: {
    node: Object,
    parent_id:{default:0},
    select_:{default:-1},
    id_item:{default:0},
  }, 
   data:() => ({
    mouse_over:-1

  }),
  mounted(){
        this.$on('select_el',(id)=>{
                this.$parent.$emit('select_el', id) 
         }),
           this.$on('click',(node)=>{
              this.$parent.$emit('click', node) 
          } )
  },

  methods:{
    get_id(){
        return String(this.parent_id) + '.' + String(this.id_item)
    },
    incriment(id){ 
        return parseInt(id) + 1
    },
    MouseOn(){ 

         this.$parent.$emit('select_el',this.get_id()) 

  
    },
    mouseleave(){

     },
     action_click(event){ 
         
        this.$parent.$emit('click',this.node)
     }

  },
  computed:{
    selected(){

      return  String(this.select_) == String(this.get_id()) 
    }
  }

};
</script>


<style scoped>

div.selected{
  background-color:greenyellow;
}
 .node-tree{
   position: absolute;
 }

li {
    border-bottom: 1px solid #E0E0E0;
    margin: 0;
    padding: 5px 5px;
}

 li:last-child {
    border-bottom: none;
}



</style>>
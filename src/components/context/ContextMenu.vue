<template>
  <div>
    <tree :tree-data="tree"></tree>
  </div>
</template>


<script>
import Tree from './TreeContext.vue'


export default {
 props: {
 
        'viewMenu':{default:true},
        'top': {default:'200px'},
        'left':{default: '0px'},
         'ext_menu':{default:'0px'},
        'element_context':{default:-1},

    },
  data:() => ({
 
      tree: {
      label: "A cool folder",
      children: [
        {
          label: "A cool sub-folder 1",
          children: [
            { label: "A cool sub-sub-folder 1" },
            { label: "A cool sub-sub-folder 2" }
          ]
        },
        { label: "This one is not that cool" }
      ]
    }, 
    // .push(this.element_context)
  }),
  components:{
    Tree
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

.center {
  text-align: center;
}

#demo {
    width: 100%;
    height: 100%;
 
}

#right-click-menu{
    background: #FAFAFA;
    border: 1px solid #BDBDBD;
    box-shadow: 0 2px 2px 0 rgba(0,0,0,.14),0 3px 1px -2px rgba(0,0,0,.2),0 1px 5px 0 rgba(0,0,0,.12);
    display: block;
    list-style: none;
    margin: 0;
    padding: 0;
    position: absolute;
    width: 250px;
    z-index: 999999;
}

#right-click-menu li {
    border-bottom: 1px solid #E0E0E0;
    margin: 0;
    padding: 5px 35px;
}

#right-click-menu li:last-child {
    border-bottom: none;
}

#right-click-menu li:hover {
    background: #1E88E5;
    color: #FAFAFA;
}

</style>
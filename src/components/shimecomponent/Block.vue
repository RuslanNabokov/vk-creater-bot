<template>
    <div   @mousedown="myDrag($event)"  
    v-on:mouseup="myDragStop()"
   
    class="block-shime row card sticky-action" 
    v-bind:style="{position:position,left:card.left,top:card.top}">
         
       
        <div class="block-head col s12"  > 
       <h6> {{card.name}} </h6>
        <a  v-on:click.stop.prevent="clickToButtonShowTools"  style="position:absolute;left: 85%;top:0%;"  class="btn-floating btn-small scale-transition"><i class=" material-icons">add</i></a>
       <div id="scale-demo" ref="navigate"  >
           <ul class="ul-menu">

        <li v-for="button in menuButtons">
            <a class="btn-floating btn-menu"  v-bind:class="button.color"  v-bind:key="button.color" >
                <i class="material-icons">{{button.text}}</i></a></li>
           </ul>
            <!-- <ul>
                <li><a class="btn-floating red"><i class="material-icons">insert_chart</i></a></li>
                <li><a class="btn-floating yellow darken-1"><i class="material-icons">format_quote</i></a></li>
                <li><a class="btn-floating green"><i class="material-icons">publish</i></a></li>
                <li><a class="btn-floating blue"><i class="material-icons">attach_file</i></a></li>
            </ul> -->
        </div>
        </div>
        <div class="block-body col s12 row" v-bind:style="{backgroundColor:card.color}"  >
        </div>
    </div>
 
</template>

<script>
export default {
    name:'block-shime',
      props: {
            card: {
            type: Object,
            default: function() {
                return {
                id: 0,
                name: 'block-shime',
                color: 'grey',
                openMenu: false,
                menu:-1,
                left:0,
                top:0,
                }
            }
            },

        },
    data:()=>({
        menuButtons : [
         {'text':'insert_chart',"color":'red'},      
            {'text':'format_quote',"color":'yellow'},      
            {'text':'publish',"color":'green'},      
        ],
        dragged:0,
        position:"relative",
  
       


    }),
    mounted(){
            let elem = this.$refs.navigate
            this.menu = M.FloatingActionButton.init(elem,{hoverEnabled: false});
            this.menu.isOpen= true
            this.menu.close()
            
    },

    methods:{
  
            clickToButtonShowTools(event){
                
                let elem = this.$refs.navigate
                var instance =  this.menu
                if (instance.isOpen){
                     instance.close()    
                }else{
                     instance.open()
                }
              
            },
    myDrag(event){
            this.dragged = 1
            this.position='absolute'
          
        },
    myDragStop(){
        this.dragged =0
        this.position='relative'
          this.card.left = this.card.left
            this.card.top = this.card.top
    },


    }
}
</script>

<style scoped>
#scale-demo{
    position: absolute;
    top:2px;
    left: calc(85%);
    height: 30px;
    width:30px;
   
}  
.block-shime{
    width:220px;
    height:300px;
    position: relative;
    left: 10px;
    top: 10px;
}
.block-head{
    background-color: #3a0e42 ;
    text-align: center;
    border-bottom: 1px solid blue-grey ;
    color: rgba(126, 160, 167, 0.7);
    }
.block-body{
    height: 100%;
}
.material-icons{
    font-size: 15px;
}
.btn-menu{
    width: 27px;
    height: 27px;
    position: relative;
    margin-top:5px;
}
.btn-menu i{
    top: -5px;
    left: 0px;
    position: absolute;
}
.ul-menu{
    margin-top:100%;
}
</style>
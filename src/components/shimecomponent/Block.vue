<template>
    <div   
     @mousedown="myDrag($event)"  v-on:mouseup="myDragStop()"
    v-bind:id = "card.id"
    class="block-shime  card sticky-action row" style="display:relative"
    v-bind:style="{position:position,left:card.left,top:card.top,margin:margin,}">
        <div class="block-head col s12 " v-bind:style="{backgroundColor:card.color}"     > 
       <h6> {{card.name}} </h6>
        <a  v-on:click.stop.prevent="clickToButtonShowTools"  style="position:absolute;left: 85%;top:0%;"  class="btn-floating btn-small scale-transition"><i class=" material-icons">add</i></a>
       <div id="scale-demo" ref="navigate"  >
           <ul class="ul-menu">

        <li v-for="button in menuButtons">
            <a class="btn-floating btn-menu"   v-bind:class="button.color"  v-bind:key="button.color" >
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
        <div class="block-body "   >
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
                menu:-1,
                left:0,
                top:0,
                width:'1px',
                
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
        position:'relative',
        margin:"0px",
    }),
    mounted(){
            let elem = this.$refs.navigate
            this.menu = M.FloatingActionButton.init(elem,{hoverEnabled: false});
            this.menu.isOpen= true
            this.menu.close()
            this.color = this.color ? this.color : "grey"
            
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
             //   this.$parent.$options.methods.myDrag(this.card.id);
             if (event.offsetY > 37){return}
             this.position = 'absolute'

             this.$parent.$emit('drag', this.card.id,event.offsetX, event.offsetY);
             
             
             console.log(event)
            },
            myDragStop(event){
             //    this.$parent.$options.methods.myDragStop(this.card.id);
             this.$parent.$emit('stopdrag', this.card.id);
             this.position = 'absolute'
             
             
            }


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
    margin:  0px;
    padding: 0px;
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
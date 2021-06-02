<template>
    <div   
     @contextmenu.prevent="contextmenu()"
    v-bind:id = "'block-' + card.id"
    v-bind:class="{'opacity-overlab-block':card.overlap_opacity}"
    v-on:click.ctrl.exact="addBlockInarraydedicated()"
    class="block-shime  card sticky-action row" 
    style="display:relative;margin:0px;padding:0px;"

    v-bind:style="{position:position,
    left:card.left,
    top:card.top,
    margin:card.margin,
    height:card.height,
    width:card.width
    
    }">
    <div class="round  round-left " v-on:click="clickRound(1)"></div>
     <div class="round  round-right" v-on:click="clickRound(2)" ></div>
    <div class="round round-bottom " v-on:click="clickRound(3)"></div>
   
        <div class="block-head col s12 " @mousedown="myDrag($event)"  v-on:mouseup="myDragStop()"  v-bind:style="{backgroundColor:card.color}"     > 
        
        <div class="resize-block" @mousedown="myResize($event)"  v-on:mouseup="myDragStop()" ></div>
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
     <p style="position:absolute; top:100%; left:  calc(50% - 10px);">{{ card.info_bot }}</p>
   <p style="position:absolute;  top: calc(50% - 10px);  left:100%;  vertical-rl; writing-mode:tb-rl;">{{ card.info_left }}</p>
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
                dedicated: -1,
                name: 'block-shime',
                color: 'grey',
                menu:-1,
                left:0,
                top:0,
                width:width,
                height:height,
                overlap_opacity:false,
               
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
        extend_menu:[{'label':'name','children':[{'label':'1','exec':'ale'}]   }],
             
    }),
    mounted(){
            let elem = this.$refs.navigate
            this.menu = M.FloatingActionButton.init(elem,{hoverEnabled: false});
            this.menu.isOpen= true
            this.menu.close()
            this.color = this.color ? this.color : "grey"
            
    },

    methods:{

            contextmenu(){ 

              
                this.$parent.$emit('right-click-to-block',this.extend_menu,this )
            },
            isdedicated(){
                return this.card.dedicated
            },
            clickRound(id){
               
                return false
            },

            addBlockInarraydedicated(){
        this.$parent.$emit('addBlockInarraydedicated',this.card.id)
            },
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
            
         
        
             this.position = 'absolute'
             this.$parent.$emit('drag', this.card.id,event.layerX, event.layerY);
            },

            myResize(event){
              this.$parent.$emit('resize',this.card.id,event.offsetX, event.offsetY)
            },

            myDragStop(event){
             //    this.$parent.$options.methods.myDragStop(this.card.id);
             this.$parent.$emit('stopdrag', this.card.id);
             this.position = 'absolute'
             
            },
            ale(){ 
                
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

.opacity-overlab-block{
    background-color:rgba(20, 60, 50, 0.2);
}
.block-shime>*{
    margin: 0px;
    padding: 0px;
}
.round {
  position: absolute;
  width: 12px;
  height: 12px;
  border: 1px solid red;
  border-radius: 50%;
  line-height: 1em;  /* подробнее про line-height */
  text-align: center; 
  background: mistyrose;
}
.round-left{
    left: -6px;
    top: calc(50% - 6px);
}

.round-bottom{ 
left: calc(50% - 6px);
bottom: -6px;
}

.round-right{
    right:-6px ;
    top:calc(50% - 6px)
}

.round-left:hover{
    left: -9px;
    top: calc(50% - 9px);
}

.round-bottom:hover{ 
left: calc(50% - 9px);
bottom: -9px;
}

.round-right:hover{
    right:-9px ;
    top:calc(50% - 9px)
}

.round:hover {
  position: absolute;
  width: 18px;
  height: 18px;
  border: 1px solid rgb(9, 29, 93);
  border-radius: 50%;
  line-height: 1em;  /* подробнее про line-height */
  text-align: center; 
  background: rgba(108, 103, 179,0.4);
 transition: 0.3s cubic-bezier(.1, .9, .9, 5.8);
}


.resize-block{
    background-color: rgb(93, 37, 37);
    width: 20px;
    height: 20px;
    position: absolute;
    left:calc(100% - 20px);
    bottom: 0px;

}

[dedicated="true"]{
   box-shadow:  rgba(50, 52, 167, 0.753) 0px 1px 4px, rgb(8, 10, 112) 0px 0px 0px 3px;
} 

</style>
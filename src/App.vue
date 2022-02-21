<template>
    <div id="view" :class="[{ collapsed: collapsed }]">    
      <div id="drawflow" ref="drawflow" />
    <sidebar-menu 
      :menu="menu"
      @update:collapsed="onCollapsed"
      @item-click="onItemClicked"
      />
  </div>
</template>
<script lang="ts">
import { Vue, Options } from 'vue-class-component';
import Drawflow from "drawflow";
import "drawflow/dist/drawflow.min.css";
import { SidebarMenu } from 'vue-sidebar-menu'
import 'vue-sidebar-menu/dist/vue-sidebar-menu.css'

@Options({
  components: {
    SidebarMenu
  }})
export default class App extends Vue {

  editor: any;
  collapsed = false;
  
  data() {
    return {
      menu:[
        {
          header: 'Node Menu',
          hiddenOnCollapse: true,
        },
        {
          title: 'Input'
        },
        {
          title: 'Sum',
          icon: 'fas fa-plus-circle'
        }
      ],
      collapsed: this.collapsed,
      editor: {},
    };
  }

  
 onItemClicked(event:PointerEvent,item:any){
   console.log('Item: ' + item);
   this.createNode(item.title);
  }
  
 createNode(node_type:string){
   switch (node_type){
     case "Sum":
        this.createSumNode();
      break;
      case "Input":
        this.createInputNode();
        break;
   }
 }
createSumNode(){
  var html = `
      <div>
      <h3>Plus:  </h3>
      <input type="text" readonly size="15px" df-Sum  />
      </div>
      `;
      var data = { "Sum" : "" }
      this.editor.addNode('Sum',2,1,30,30,'Sum',data,html,false);
}

createInputNode(){
   var html = `
      <div>
      <h3>Input Data: </h3>
      <input type="text" size="15px" df-input />
      </div>
      `;
      var data = { "input" : '' }
      this.editor.addNode('Input',0,1,30,30,'input',data,html,false);
}

 


  onCollapsed(c:boolean) {
    console.log("onCollapse");
    this.collapsed = c;
  }

    mounted() {
     this.editor = new Drawflow(this.$refs.drawflow as HTMLElement, Vue, this);
     this.editor.reroute = true;
     this.editor.drawflow = {
        drawflow: {
          Home: {
            data: {
              "1": {
          id: 1,
          name: "welcome",
          data: {},
          class: "welcome",
          html:
            '\n    <div>\n      <div class="title-box">👏 Welcome!!</div>\n      <div class="box">\n        <p>Simple flow library <b>demo</b>\n        <a href="https://github.com/jerosoler/Drawflow" target="_blank">Drawflow</a> by <b>Jero Soler</b></p><br>\n\n        <p>Multiple input / outputs<br>\n           Data sync nodes<br>\n           Import / export<br>\n           Modules support<br>\n           Simple use<br>\n           Type: Fixed or Edit<br>\n           Events: view console<br>\n           Pure Javascript<br>\n        </p>\n        <br>\n        <p><b><u>Shortkeys:</u></b></p>\n        <p>🎹 <b>Delete</b> for remove selected<br>\n        💠 Mouse Left Click == Move<br>\n        ❌ Mouse Right == Delete Option<br>\n        🔍 Ctrl + Wheel == Zoom<br>\n        📱 Mobile support<br>\n        ...</p>\n      </div>\n    </div>\n    ',
          typenode: false,
          inputs: {},
          outputs: {},
          pos_x: 50,
          pos_y: 50
        }
            }
          }
        }
      }
      this.editor.start(); 

      //Event managment: 
      let inputsClasses:Array<string> = [];
      this.editor.on('connectionCreated', (data:any) =>{
        if (data.input_class == 'input_1' || data.input_class == 'input_2') inputsClasses.unshift(data.output_id);
        if (inputsClasses.length == 2)  {
          const inputNode1 = Number.parseInt(this.editor.getNodeFromId(inputsClasses[0]).data.input);
          const inputNode2 = Number.parseInt(this.editor.getNodeFromId(inputsClasses[1]).data.input);
          let Sum:number = inputNode1 + inputNode2;
          this.editor.updateNodeDataFromId(data.input_id, {'Sum': Sum.toString()})
        }
      })

  }

}


</script>

<style>
#drawflow {
  height: 95vh;
  width: 100%;
  border: 1px solid black;
}
#view {
  padding-left: 350px;
}
#view.collapsed{
  padding-left: 65px;
}


</style>

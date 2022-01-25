<template>
  <sidebar-menu :menu="menu" />
  <div id="drawflow"></div>
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
  
  data() {
    return {
      menu:[
        {
          header: 'Node Menu',
          hiddenOnCollapse: true,
        },
        {
          href: '/',
          title: 'Sumar',
          icon: 'fas fa-plus-circle'
        }
      ]
    }
  }
  mounted() {
    this.$nextTick(() => {
      let id = document.getElementById("drawflow") as HTMLElement;
      const editor = new Drawflow(id, Vue, this);
      editor.reroute = true;
      editor.drawflow = {
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
      editor.start();
    });
  }

}
</script>

<style>
#drawflow {
  height: 95vh;
  width: 67vw;
  margin-left: 30vw;

  position: relative;
  border: 1px solid black;
}

</style>

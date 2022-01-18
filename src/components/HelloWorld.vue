<template>
<div id="drawflow"></div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Drawflow from 'drawflow';
import 'drawflow/dist/drawflow.min.css'

@Options({
  props: {
    msg: String,
  },
})
export default class HelloWorld extends Vue {
  msg!: string;
  
  

  mounted() {
   this.$nextTick(() => {
    let id = document.getElementById("drawflow") as HTMLElement;
    console.log(id)
    const editor = new Drawflow(id, Vue, this);
    editor.start();
    

    
   })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

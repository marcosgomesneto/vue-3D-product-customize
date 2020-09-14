<template>
  <div id="app">
    <b-container>
      <ToolBar v-model="bottleOptions"/>
      <Label v-model="canvasLabel" :bottleOptions="bottleOptions"/>
      <Bottle :canvasLabel="canvasLabel"/>
      
    </b-container>
  </div>
</template>

<script>
import Bottle from "./components/Bottle.vue"
import Label from "./components/Label.vue"
import ToolBar from "./components/ToolBar.vue"

export default {
  name: 'App',
  components: {
    Bottle,
    Label,
    ToolBar
  },
  data(){
    return{
      canvasLabel: null,
      bottleOptions: {
        title: 'Sua marca teste aqui'
      }
    }
  }
}
</script>

<style>
body{
  margin: 0;
  padding: 0;
}
#app {

}
</style>

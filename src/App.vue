<template>
  <div id="app">
    <b-container fluid class="p-0">
      <ToolBar v-model="bottleOptions"/>
      <div id="wrapEdit">
        <Label v-model="canvasLabel" :bottleOptions="bottleOptions"/>
        <Bottle :canvasLabel="canvasLabel" :bottleOptions="bottleOptions.bottle"/>
      </div>
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
        label: {
          title: 'Sua marca teste aqui',
          titleColor: '#000000',
          subtitle: 'Suplemento alimentar de vitaminas e minerais em cápsulas',
          subtitleColor: '#000033',
          capsNumber: 60
        },
        bottle: {
          bottleColor: '#00FFFF',
          bottleLidColor: '#000'
        }
      }
    }
  }
}
</script>

<style lang="scss">
@import '~bootstrap';
@import '~bootstrap-vue';
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;700&display=swap');
body #app{
    font-family: 'Roboto';
}
body{
  margin: 0;
  padding: 0;
}
#wrapEdit{
  margin-left: 300px;
}
#app {

}
</style>

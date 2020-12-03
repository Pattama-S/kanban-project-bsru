<template>
  <div id="app">
    <Kanban :data="data"/>
  </div>
</template>

<script>
import Kanban from "./components/Kanban"

export default {
  name: 'App',
  components: {
    Kanban
  },
  data(){
    return{
      data:[
        {
          name:"todo",
          color:"#e3ec58",
          task:[]
        },
         {
          name:"doing",
          color:"#58ec78",
          task:[]
        },
         {
          name:"done",
          color:"#4be7e3",
          task:[]
        }
      ]
    }
  }
}
</script>

<style>
#app {
  height: 100vh;
}
</style>

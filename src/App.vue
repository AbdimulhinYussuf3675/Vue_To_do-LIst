<template>
    <todo-list></todo-list>
</template>

<script>
import todoList from './components/todoList.vue'

export default {
  name: 'App',
  components: {
    todoList,
  }
}
</script>

<style lang="scss">
#app {
  font-size: 24px;
  background-color: rgb(245, 244, 244);
}


* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  font-family: Rubik Sans;
}
</style>

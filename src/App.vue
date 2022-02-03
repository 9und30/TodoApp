<template>

<div id="App">
  <TodoApp class="todo" />
</div>

</template>

<script>

import TodoApp from './components/TodoApp.vue'

export default {
  name: 'App',
  components: {
    TodoApp
  }
}

</script>


<style>

*, ::before, ::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#App {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  font-family: 'Poppins', sans-serif;
}

</style>

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
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  font-family: 'Poppins', sans-serif;
}

.darkmode {
  background-color: #333;
}

.lightmode {
  background-color: white;
}

</style>

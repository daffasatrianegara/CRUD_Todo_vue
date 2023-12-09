<template>
  <h1 class="text-center mt-2">Data todo</h1>
  <div class="data d-flex justify-content-center">
    <DataTodos />
  </div>
</template>

<script>
import allTodos from './components/allTodos.vue';


export default {
  name: 'App',
  components: {
    DataTodos : allTodos,
  }
}
</script>
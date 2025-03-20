<script setup>
const { data } = useFetch('http://localhost:8000/api/todos/');

</script>

<template>
  <div>
    <h1>Lista de Tareas</h1>
    <p>{{ data }}</p>
  </div>
</template>

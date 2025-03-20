<script setup>
const { data: todos } = await useFetch('http://localhost:8000/api/todos/');

const updateTodo = async (todo) => {
  try {
    await $fetch(`http://localhost:8000/api/todos/${todo.id}/`, {
      method: 'PUT',
      body: JSON.stringify({ title: todo.title, completed: todo.completed }),
      headers: { 'Content-Type': 'application/json' }
    });
  } catch (error) {
    console.error('Error al actualizar tarea:', error);
  }
};
</script>

<template>
  <div>
    <h1>Lista de Tareas</h1>
    <p>{{ todos }}</p>
    <div v-for="todo in todos">
      <input type="checkbox" v-model="todo.completed" @change="updateTodo(todo)" />
      <span>{{ todo.title }}</span>
      <button @click="deleteTodo(todo.id)">Eliminar</button>
    </div>
  </div>
</template>

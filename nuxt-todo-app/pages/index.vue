<script setup>
const newTodoTitle = ref('');
const { data: todos } = await useFetch('http://localhost:8000/api/todos/');

const addTodo = async () => {
  if (!newTodoTitle.value) return;

  try {
    const response = await $fetch('http://localhost:8000/api/todos/', {
      method: 'POST',
      body: JSON.stringify({ title: newTodoTitle.value, completed: false }),
      headers: { 'Content-Type': 'application/json' }
    });
    todos.value.push(response);
    newTodoTitle.value = '';
  } catch (error) {
    console.error('Error al agregar tarea:', error);
  }
};

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

const deleteTodo = async (id) => {
  try {
    await $fetch(`http://localhost:8000/api/todos/${id}/`, { method: 'DELETE' });
    todos.value = todos.value.filter(todo => todo.id !== id);
  } catch (error) {
    console.error('Error al eliminar tarea:', error);
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
    <form @submit.prevent="addTodo">
      <input type="text" v-model="newTodoTitle" placeholder="Nueva tarea" />
      <button type="submit">Agregar</button>
    </form>
  </div>
</template>

<style scoped>
button {
  margin-left: 10px;
}
</style>
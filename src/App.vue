<template>
  <div class="container mt-4">
    <form @submit.prevent="addTodo">
      <fieldset class="mb-3">
        <div class="input-group">
          <input
            v-model="newTodo"
            type="text"
            class="form-control"
            placeholder="Tâche à effectuer"
            aria-label="Tâche à effectuer"
          />
          <button
            class="btn btn-primary"
            type="submit"
            :disabled="newTodo == 0"
          >
            Ajouter
          </button>
        </div>
      </fieldset>
    </form>

    <div v-if="todos.length == 0" class="alert alert-info">
      Vous n'avez pas de tâche à faire :(
    </div>
    <div v-else>
      <ul class="list-group">
        <li
          v-for="todo in sortedTodos()"
          :key="todo.date"
          :class="['list-group-item', { completed: todo.completed }]"
        >
          <label class="form-check-label">
            <input
              type="checkbox"
              class="form-check-input me-2"
              v-model="todo.completed"
            />
            {{ todo.title }}
          </label>
        </li>
      </ul>
      <div class="form-check mt-3">
        <input
          type="checkbox"
          class="form-check-input"
          id="hideCompleted"
          v-model="hideCompleted"
        />
        <label class="form-check-label" for="hideCompleted">
          Masquer les tâches complétées
        </label>
      </div>
    </div>
  </div>
</template>

<script setup>
// Étape 1: créer un tableau qui va contenir les todos List
import { ref } from "vue";

const newTodo = ref("");
const hideCompleted = ref(false);
const todos = ref([
  {
    title: "Tache de test",
    completed: true,
    date: 1,
  },
  {
    title: "Tache à faire",
    completed: false,
    date: 2,
  },
]);
const addTodo = () => {
  todos.value.push({
    title: newTodo.value,
    completed: false,
    date: Date.now(),
  });
  newTodo.value = "";
};

const sortedTodos = () => {
  const sortedTodos = todos.value.toSorted((a, b) =>
    a.completed > b.completed ? 1 : -1
  );
  if (hideCompleted.value == true) {
    return sortedTodos.filter((t) => t.completed == false);
  }
  return sortedTodos;
};
</script>

<style>
.completed {
  opacity: 0.5;
  text-decoration: line-through;
}

.input-group .form-control {
  border-right: 0;
}

.input-group .btn {
  border-left: 0;
}
</style>

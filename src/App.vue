<template>
  <div class="app">
    <div class="card">
      <h1 class="title">📝 Yuk Diatur</h1>
      
      <!-- Input Section -->
      <div class="input-section">
        <input
          v-model="newTodo"
          @keyup.enter="addTodo"
          placeholder="Silahkan..."
          class="todo-input"
        />
        <button @click="addTodo" class="add-btn" :disabled="!newTodo.trim()">
          <span class="plus-icon">+</span>
        </button>
      </div>

      <!-- Filter Section -->
      <div class="filter-section">
        <button
          @click="setFilter('all')"
          :class="{ active: currentFilter === 'all' }"
          class="filter-btn"
        >
          Cek Semua
        </button>
        <button
          @click="setFilter('uncompleted')"
          :class="{ active: currentFilter === 'uncompleted' }"
          class="filter-btn"
        >
          Cek Belum Siap
        </button>
      </div>

      <!-- Todo List -->
      <transition-group name="fade" tag="ul" class="todo-list">
        <li
          v-for="todo in filteredTodos"
          :key="todo.id"
          :class="{ completed: todo.completed }"
          class="todo-item"
        >
          <label class="checkbox-container">
            <input
              type="checkbox"
              v-model="todo.completed"
              class="checkbox"
            />
            <span class="checkmark"></span>
            <span class="todo-text" :class="{ 'completed': todo.completed }">
              {{ todo.text }}
            </span>
          </label>
          <button @click="removeTodo(todo.id)" class="delete-btn">
            <span class="trash-icon">Batalkan</span>
          </button>
        </li>
      </transition-group>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      todos: [
        { id: 1, text: "Bermain bola", completed: false },
        { id: 2, text: "Joging", completed: false },
        { id: 3, text: "Badminton", completed: false }
      ],
      currentFilter: "all",
      nextId: 4
    };
  },
  computed: {
    filteredTodos() {
      if (this.currentFilter === "uncompleted") {
        return this.todos.filter((todo) => !todo.completed);
      }
      return this.todos;
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() === "") return;
      this.todos.push({
        id: this.nextId++,
        text: this.newTodo,
        completed: false
      });
      this.newTodo = "";
    },
    cancelTodo() {
      this.newTodo = "";
    },
    toggleComplete(id) {
      const todo = this.todos.find((todo) => todo.id === id);
      if (todo) todo.completed = !todo.completed;
    },
    removeTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    setFilter(filter) {
      this.currentFilter = filter;
    }
  }
};
</script>

<style>
:root {
  --dark: #12343d;
  --gray: #243c47;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Times New Roman', Times, serif;
}

.app {
  min-height: 100vh;
  background: linear-gradient(#568ee3 0%, #ccd4e2 100%);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.card {
  background: rgb(97, 140, 214);
  border-radius: 16px;
  padding: 25px;
  width: 100%;
  max-width: 500px;
}

.title {
  color: black;
  text-align: center;
  margin-bottom: 23px;
  font-size: 30px;
}

/* style untuk Input Section */
.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 25px;
  position: relative;
}

.todo-input {
  flex: 1;
  padding: 13px 16px;
  border-radius: 10px;
  font-size: 15px;
}

.add-btn {
  color: white;
  background-color: #2e332e;
  border-radius: 10px;
  padding: 0 20px;
  display: flex;
  align-items: center;
}

/* style untuk Filter Section */
.filter-section {
  display: flex;
  margin-bottom: 20px;
  background: #fef8f8;
  padding: 6px;
  border-radius: 50px;
}

.filter-btn {
  flex: 1;
  padding: 8px 12px;
  border: none;
  border-radius: 50px;
  font-size: 15px;
  font-weight: 500;
  background: transparent;
  color: black;
}

/* style untuk Todo List */
.todo-list {
  list-style: none;
  margin-bottom: 20px;
}

.todo-item {
  display: flex;
  align-items: center;
  padding: 14px 16px;
  margin-bottom: 10px;
  background: white;
  border-radius: 8px;
}

/* style untuk Custom Checkbox */
.checkbox-container {
  display: flex;
  align-items: center;
  position: relative;
  user-select: none;
  flex-grow: 1;
}

.checkbox {
  position: absolute;
}

.checkmark {
  position: relative;
  height: 20px;
  width: 20px;
  background-color: white;
  border: 2px solid var(--gray);
  border-radius: 4px;
  margin-right: 10px;
}

/* style untuk Todo Text */
.todo-text {
  flex: 1;
  font-size: 16px;
  color: black;
}

.completed .todo-text {
  text-decoration: line-through;
  color: gray;
}

/* style untuk delete button */
.delete-btn {
  background: none;
  border: none;
  color: black;
  font-size: 16px;
  padding: 4px;
}
</style>
<template>
  <div class="todo-app">
    <div class="header">
      <h1>My Vue Todo</h1>
      <span>
        <input
          v-model="newTodo"
          placeholder="Add a new todo..."
          @keyup.enter="addTodo"
        />
        <button @click="addTodo">Add</button>
      </span>
    </div>

    <div class="filters">
      <button
        @click="filterTodos('all')"
        :class="{ active: currentFilter === 'all' }"
      >
        All
      </button>
      <button
        @click="filterTodos('completed')"
        :class="{ active: currentFilter === 'completed' }"
      >
        Completed
      </button>
      <button
        @click="filterTodos('ongoing')"
        :class="{ active: currentFilter === 'ongoing' }"
      >
        Ongoing
      </button>
    </div>

    <div class="todos-list">
      <div
        v-for="todo in filteredTodos"
        :key="todo.id"
        class="todo-item"
        :class="{ completed: todo.completed }"
        @transition="animateTodo"
      >
        <span>
       
          <p>{{ todo.title }}</p>
        </span>
        <div class="actions">
          <button @click="editTodo(todo)">Edit</button>
          <button @click="deleteTodo(todo)">Delete</button>
             <input
            type="checkbox"
            v-model="todo.completed"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      todos: [],
      editedTodoId: null,
      editedTodoTitle: "",
      currentFilter: "all",
      completed: true
    };
  },

  computed: {
    filteredTodos() {
      if (this.currentFilter === "all") {
        return this.todos;
      } else if (this.currentFilter === "completed") {
        return this.todos.filter((todo) => todo.completed);
      } else {
        return this.todos.filter((todo) => !todo.completed);
      }
    },
  },

  mounted() {
    // Replace with your actual data fetching method
    this.todos = [
      { id: 1, title: "Testing Microphone", completed: this.completed },
      { id: 2, title: "Finish project report", completed: this.completed },
    ];
  },

  methods: {
    filterTodos(filter) {
      this.currentFilter = filter;
    },

    addTodo() {
      if (this.newTodo.trim() !== "") {
        this.todos.push({
          id: Date.now(),
          title: this.newTodo,
          completed: false,
        });
        this.newTodo = "";
      }
    },

    editTodo(todo) {
      this.editingTodoId = todo.id;
      this.editedTodoTitle = todo.title;
    },

    updateTodo(todo) {
      if (this.editedTodoTitle.trim() !== "") {
        todo.title = this.editedTodoTitle;
        this.editingTodoId = null;
        this.editedTodoTitle = "";
      }
    },

    updateCompleted(todo) {
      todo.completed = !todo.completed; // Toggle the completed state
    },

    // deleteTodo(id) {
    //   const confirmDelete = confirm("Are you sure you want to delete this todo?");
    //   if (confirmDelete) {
    //     this.todos = this.todos.filter((todo) => {
    //       return todo.id !== id;
    //     })
    //   }
    //   console.log(this.todos);
    //   console.log(id);

    // },
    deleteTodo(tod) {
      const filteredTodos = this.todos.filter((todo) => todo.id !== tod.id);
      this.todos = filteredTodos;
    },

    animateTodo(el, done) {
      el.classList.add("todo-item-anim");
      setTimeout(() => {
        el.classList.remove("todo-item-anim");
        done();
      }, 300);
    },
  },
};
</script>


<style scoped>
.todo-app {
  font-family: Arial, sans-serif;
  margin: 0 auto;
  width: 800px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
}

.filters {
  display: flex;
  justify-content: space-evenly;
  margin: 10px;
}

.filters button {
  padding: 5px 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: #eee;
  cursor: pointer;
}

.filters button.active {
  background-color: #ddd;
}

.todos-list {
  padding: 20px;
  border-radius: 4px;
  background-color: #fff;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.todo-item.completed {
  background-color: #eee;
  margin-bottom: 10px;
}

.todo-item p {
  margin: 0;
  font-size: 16px;
}

.actions {
  display: flex;
  gap: 10px;
}

.actions button {
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  background-color: #ddd;
  cursor: pointer;
}

.actions button:hover {
  background-color: #ccc;
}

span{
  display: inline-flex;
}
</style>


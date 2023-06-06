<template>
  <div class="container" style="max-width: 540px">
    <div class="p-5 mb-4 mt-4 bg-dark rounded-3">
      <h1 class="d-flex justify-content-center text-white">{{ title }}</h1>
    </div>

    <TodoCard title="Add Todo">
      <TodoAdd @addTodo="addTodoHandler" />
    </TodoCard>

    <template v-if="todos.length !== 0">
      <TodoCard title="Todos not done">
        <Todos>
          <TodoItem
            :todo="todo"
            v-for="todo in todosNotDone"
            :key="todo.id"
            @doneChange="todoDoneChangeHandler"
            @delete="todoDeleteHandler"
          />
        </Todos>
      </TodoCard>

      <TodoCard title="Todos done">
        <Todos>
          <TodoItem
            :todo="todo"
            v-for="todo in todosDone"
            :key="todo.id"
            @doneChange="todoDoneChangeHandler"
            @delete="todoDeleteHandler"
          />
        </Todos>
      </TodoCard>
    </template>
    <template v-else>
      <TodoCard title="No Todos"></TodoCard>
    </template>
  </div>
</template>

<script>
import { todos } from "./dummy-data";
import TodoItem from "./components/todos/TodoItem.vue";
import Todos from "./components/todos/Todos.vue";
import TodoAdd from "./components/todos/TodoAdd.vue";
import TodoCard from "./components/todos/TodoCard.vue";

export default {
  components: { TodoItem, Todos, TodoAdd, TodoCard },
  name: "App",
  data() {
    return {
      title: "Vue 2do",
      todos: [],
    };
  },
  computed: {
    todosDone() {
      return this.todos.filter((item) => item.done);
    },
    todosNotDone() {
      return this.todos.filter((item) => !item.done);
    },
  },
  methods: {
    // Handlers
    todoDoneChangeHandler(evt) {
      const index = this.todos.findIndex((item) => item.id === evt.id);
      this.todos[index].done = evt.done;
      this.todosUpdated();
      //this.save();
    },
    todoDeleteHandler(id) {
      if (window.confirm(`Do you want delete Todo ID: ${id}?`)) {
        const index = this.todos.findIndex((item) => item.id === id);
        this.todos.splice(index, 1);
        this.todosUpdated();
      }
      //this.save();
    },
    addTodoHandler(title) {
      let maxID = 0;
      this.todos.forEach((item) => {
        if (maxID <= item.id) {
          maxID = item.id + 1;
        }
      });
      this.todos.unshift({ id: maxID, title: title, done: false });
      this.todosUpdated();
      //this.save();
    },

    // Speichern und Laden
    save() {
      window.localStorage.setItem("TODOS", JSON.stringify(this.todos));
    },
    load() {
      const dataJSON = window.localStorage.getItem("TODOS");
      if (dataJSON) {
        const data = JSON.parse(dataJSON);
        this.todos = data;
      }
    },

    todosUpdated() {
      this.save();
    },
  },

  mounted() {
    console.log("mounted");
    console.log(this.todos);
    this.load();
  },
  created() {
    console.log("created");
  },
  updated() {
    console.log("updated");
  },
};
</script>
<style></style>

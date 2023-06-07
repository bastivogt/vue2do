<template>
  <div class="container" style="max-width: 540px">
    <div class="p-5 mb-4 mt-4 bg-body-secondary rounded-3">
      <div class="d-flex flex-row justify-content-center align-items-center">
        <img src="./assets/myTodoListIcon.svg" class="logo me-2" />
        <h1 class="d-flex justify-content-center">{{ title }}</h1>
      </div>
    </div>

    <TodoCard title="Todo hinzufügen">
      <TodoAdd
        buttonTitle="Neues Todo"
        placeholder="Todo Titel"
        @addTodo="addTodoHandler"
      />
    </TodoCard>

    <template v-if="todos.length !== 0">
      <TodoCard title="Todos nicht erledigt">
        <template v-if="todosNotDone.length !== 0">
          <Todos>
            <TodoItem
              :todo="todo"
              v-for="todo in todosNotDone"
              :key="todo.id"
              @doneChange="todoDoneChangeHandler"
              @delete="todoDeleteHandler"
            />
          </Todos>
        </template>
        <template v-else>
          <p>Keine Todos vorhanden</p>
        </template>
      </TodoCard>

      <TodoCard title="Todos erledigt">
        <template v-if="todosDone.length !== 0">
          <Todos>
            <TodoItem
              :todo="todo"
              v-for="todo in todosDone"
              :key="todo.id"
              @doneChange="todoDoneChangeHandler"
              @delete="todoDeleteHandler"
            />
          </Todos>
        </template>
        <template v-else>
          <p>Keine Todos vorhanden</p>
        </template>
      </TodoCard>
    </template>

    <template v-else>
      <TodoCard title="Keine Todos vorhanden"></TodoCard>
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
    },
    todoDeleteHandler(id) {
      if (window.confirm(`Willst du wirklich [Todo ID: ${id}] löschen?`)) {
        const index = this.todos.findIndex((item) => item.id === id);
        this.todos.splice(index, 1);
      }
    },
    addTodoHandler(title) {
      let maxID = 1;
      this.todos.forEach((item) => {
        if (maxID <= item.id) {
          maxID = item.id + 1;
        }
      });
      this.todos.unshift({ id: maxID, title: title, done: false });
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
  },

  watch: {
    todos: {
      handler(newVal, oldVal) {
        console.log("watcher todos");
        console.log(newVal);
        this.save();
      },
      deep: true,
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

<style scoped>
.logo {
  width: 40px;
  height: auto;
}
</style>

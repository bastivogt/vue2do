<template>
  <div class="input-group">
    <input
      type="text"
      placeholder="Todo"
      class="form-control"
      v-model="inputText"
      @keyup.enter="addTodoHandler"
    />
    <button class="btn btn-primary" @click="addTodoHandler">
      {{ buttonTitle }}
    </button>
  </div>
</template>
<script>
export default {
  name: "TodoAdd",
  data() {
    return {
      inputText: "",
    };
  },
  props: {
    buttonTitle: {
      type: String,
      default: "Add Todo",
    },
  },
  methods: {
    addTodoHandler() {
      if (this.inputText !== "") {
        this.$emit("addTodo", this.inputText);
        this.inputText = "";
      }
    },
  },
};
</script>
<style scoped></style>

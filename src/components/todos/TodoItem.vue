<template>
  <li class="list-group-item d-flex align-items-center">
    <div class="w-100 d-flex align-items-center">
      <div>
        <input
          class="form-check-input me-1"
          type="checkbox"
          :checked="todo.done"
          @change="checkedHandler"
          :id="checkBoxID"
        />
      </div>
      <label
        class="form-check-label w-auto"
        :for="checkBoxID"
        :style="doneStyle"
        >{{ todo.title }}</label
      >
    </div>
    <div class="flex-shrink-1">
      <button class="btn btn-danger btn-sm" @click="deleteHandler">x</button>
    </div>
  </li>
</template>

<script>
export default {
  name: "TodoItem",
  data() {
    return {};
  },
  props: {
    todo: {
      type: Object,
      required: true,
    },
  },
  computed: {
    doneStyle() {
      if (this.todo.done) {
        return {
          textDecoration: "line-through",
        };
      }
      return {
        textDecoration: "none",
      };
    },
    checkBoxID() {
      return `${this.todo.id}-${this.todo.title}`;
    },
  },
  methods: {
    checkedHandler(evt) {
      //console.log(evt.target.checked);
      this.$emit("doneChange", { id: this.todo.id, done: evt.target.checked });
    },
    deleteHandler(evt) {
      this.$emit("delete", this.todo.id);
    },
  },
};
</script>

<style scoped></style>

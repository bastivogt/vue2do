<template>
  <div class="card mb-4">
    <div class="card-body">
      <template v-if="title">
        <h5 class="card-title">{{ title }}</h5>
      </template>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoCard",
  props: {
    title: {
      type: String,
    },
  },
};
</script>

<style scoped></style>

<template>
  <div>
    <h1>{{ msg }}</h1>
    <todo-list />
  </div>
</template>

<script>
import Counter from "./Counter.vue";
import TodoList from "./TodoList.vue";

export default {
  components: {
    Counter,
    TodoList,
  },
  props: {
    msg: {
      type: String,
      default: "",
    },
  },
};
</script>

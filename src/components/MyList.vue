<template>
  <ul class="todo-main">
    <MyItem v-for="todo in todos" :key="todo.id" :todo="todo" />
  </ul>
</template>

<script>
import MyItem from "./MyItem.vue";
export default {
  name: "MyList",
  components: { MyItem },

  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style>
.todo-main {
  padding-left: 0;
  margin: 15px auto;
  border: 1px solid rgb(204, 201, 201);
  border-radius: 2px;
  /* margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0; */
}
.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>
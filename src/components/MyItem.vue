<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @change="handleCheck(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input
        v-show="todo.isEdit"
        type="text"
        :value="todo.title"
        @blur="handleBlur(todo, $event)"
        ref="todoChange"
      />
    </label>

    <button class="btn btn-danger" @click="DeleteTodo(todo.id)">删除</button>
    <button
      v-show="!todo.isEdit"
      class="btn btn-edit"
      @click="handleEdit(todo)"
    >
      编辑
    </button>
  </li>
</template>

<script>
import pubsub from "pubsub-js";
export default {
  name: "MyItem",
  //接收todo对象
  props: ["todo"],
  methods: {
    handleCheck(id) {
      //改变App组件的对象
      //this.changDone(id),
      this.$bus.$emit("changDnoe", id);
    },
    DeleteTodo(id) {
      if (confirm("确定删除吗")) {
        //this.ListDelete(id);
        //this.$bus.$emit("ListDelete", id);
        pubsub.publish("deleteTodo", id);
      }
    },
    handleEdit(todo) {
      todo.isEdit = true;
      this.$nextTick(() => {
        this.$refs.todoChange.focus();
      });

      console.log(todo);
    },
    handleBlur(todo, e) {
      todo.isEdit = false;
      if (!e.target.value) return alert("输入不能为空！！！");
      this.$bus.$emit("updateTdo", todo.id, e.target.value);
      console.log(todo);
    },
  },
};
</script>

<style>
li {
  list-style-type: none;
  height: 36px;
  padding: 0 5px;

  line-height: 36px;
  border-bottom: 1px solid #ddd;
}
li label {
  float: left;
  cursor: pointer;
}
li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}
li button {
  float: right;
  display: none;
  margin-top: 6px;
}
li::before {
  content: initial;
}
li:last-child {
  border-bottom: none;
}
li:hover {
  background-color: #ddd;
}
li:hover button {
  display: block;
}
</style>
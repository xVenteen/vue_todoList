<template>
  <div class="todo-footer" v-show="total">
    <label for="">
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ doneTotal }}</span
      >/全部{{ total }}
    </span>
    <button class="btn btn-danger" @click="clearAllTodo">
      清除已完成的任务
    </button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props: ["todos"],
  computed: {
    isAll: {
      get() {
        return this.doneTotal === this.total && this.total > 0;
      },
      set(value) {
        // this.checkAllTodo(value);
        this.$emit("checkAllTodo", value);
      },
    },
    total() {
      return this.todos.length;
    },
    doneTotal() {
      /* let i = 0;      
      this.todos.forEach((element) => {
        if (element.done) i++;
      });
      return i; */
      return this.todos.reduce((pre, todo) => pre + (todo.done ? 1 : 0), 0);
    },
  },
  methods: {
    /*  checkAll(e) {
      this.checkAllTodo(e.target.checked);
    }, */
    clearAllTodo() {
      if (confirm("确定删除吗")) {
        // this.clearAll();
        this.$emit("clearAll");
      }
    },
  },
};
</script>

<style>
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}
.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}
.todo-footer label input {
  position: relative;
  top: -1px;
}
.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
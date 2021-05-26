<template>
  <section class="todoapp">
    <TodoHeader @add="add"></TodoHeader>
    <TodoMain
      :list="list"
      :type="type"
      @toggleCompleted="toggleCompleted"
      @del="del"
      @edit="edit"
      @allDone="allDone"
    ></TodoMain>
    <TodoFooter :list="list" :type="type" @filter="filter" @clearCompleted="clearCompleted"></TodoFooter>
  </section>
</template>

<script>
import TodoHeader from './components/TodoHeader'
import TodoMain from './components/TodoMain'
import TodoFooter from './components/TodoFooter'
export default {
  // 注册组件
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter,
  },
  data() {
    return {
      list: JSON.parse(localStorage.getItem('todoList')) || [],
      type: 'all',
    }
  },
  methods: {
    // 切换完成状态
    toggleCompleted(id) {
      const item = this.list.find(item => item.id === id)
      item.isCompleted = !item.isCompleted
    },
    // 删除
    del(id) {
      this.list = this.list.filter(item => item.id !== id)
    },
    // 修改
    edit(id, title) {
      this.list.find(item => item.id === id).title = title
    },
    // 全选控制单选
    allDone(value) {
      this.list.forEach(item => (item.isCompleted = value))
    },
    // 增加
    add(title) {
      this.list.unshift({
        id: Date.now(),
        title,
        isCompleted: false,
      })
    },
    // 切换
    filter(type) {
      this.type = type
    }, 
    // 删除完成
    clearCompleted () {
      this.list = this.list.filter(item => !item.isCompleted)
    }
  },
  watch: {
    list: {
      deep: true,
      handler (newValue) {
        localStorage.setItem('todoList', JSON.stringify(newValue))
      }
    }
  }
}
</script>

<style></style>

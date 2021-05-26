<template>
  <section class="main">
    <input id="toggle-all" class="toggle-all" type="checkbox" v-model="allDone"/>
    <label for="toggle-all">Mark all as complete</label>
    <ul class="todo-list">
      <li
        :class="{ completed: item.isCompleted, editing: item.id === currentId }"
        v-for="item in showList"
        :key="item.id"
      >
        <div class="view">
          <input
            class="toggle"
            type="checkbox"
            :checked="item.isCompleted"
            @change="toggleCompleted(item.id)"
          />
          <label @dblclick="showEdit(item)">{{ item.title }}</label>
          <button class="destroy" @click="del(item.id)"></button>
        </div>
        <input class="edit" v-model="currentTitle" v-focus @keyup.esc="cancel" @keyup.enter="edit"/>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  // 双击自动聚焦
  directives: {
    focus: {
      // 指令的定义
      update: function(el) {
        el.focus()
      },
    },
  },
  data() {
    return {
      currentId: '',
      currentTitle: '',
    }
  },
  props: {
    list: {
      type: Array,
      required: true,
    },
    type: {
      type: String,
      required: true
    }
  },
  methods: {
    // 切换完成状态
    toggleCompleted(id) {
      this.$emit('toggleCompleted', id)
    },
    // 删除
    del(id) {
      this.$emit('del', id)
    },
    // 双击显示输入框
    showEdit(item) {
      this.currentId = item.id
      this.currentTitle = item.title
    },
    // 取消编辑
    cancel () {
      this.currentId = '',
      this.currentTitle = ''
    },
    // 完成编辑
    edit () {
      this.$emit('edit', this.currentId, this.currentTitle)
      this.currentId = '',
      this.currentTitle = ''
    }
  },
  computed: {
    // 全选
    allDone: {
      get () {
        return this.list.every(item => item.isCompleted)
      },
      set (newValue) {
        this.$emit('allDone', newValue)
      }
    },
    // type筛选
    showList () {
      if (this.type === 'active') {
        return this.list.filter(item => !item.isCompleted)
      }else if (this.type === 'completed') {
        return this.list.filter(item => item.isCompleted)
      }else {
        return this.list
      }
    }
  }
}
</script>

<style></style>

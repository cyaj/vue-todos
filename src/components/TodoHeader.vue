<template>
  <header class="header">
    <h1>todos</h1>
    <input class="new-todo" placeholder="What needs to be done?" autofocus v-model.trim="title" @keyup.enter="add"/>
  </header>
</template>

<script>
export default {
  data () {
    return {
      title: ''
    }
  },
  methods: {
    add () {
      if (!this.title) return 
      this.$emit('add', this.title)
      this.title = ''
    }
  }
}
</script>

<style></style>

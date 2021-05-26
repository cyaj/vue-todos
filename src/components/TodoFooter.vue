<template>
  <footer class="footer" v-if="list.length">
    <span class="todo-count"
      ><strong>{{ leftCount }}</strong> item left</span
    >
    <ul class="filters">
      <li>
        <a
          :class="{ selected: type === 'all' }"
          href="#/"
          @click.prevent="filter('all')"
          >All</a
        >
      </li>
      <li>
        <a
          :class="{ selected: type === 'active' }"
          href="#/active"
          @click.prevent="filter('active')"
          >Active</a
        >
      </li>
      <li>
        <a
          :class="{ selected: type === 'completed' }"
          href="#/completed"
          @click.prevent="filter('completed')"
          >Completed</a
        >
      </li>
    </ul>
    <button v-show="showClear" class="clear-completed" @click="clearCompleted">Clear completed</button>
  </footer>
</template>

<script>
export default {
  props: {
    list: {
      type: Array,
      required: true,
    },
    type: {
      type: String,
      default: 'all',
    },
  },
  methods: {
    filter(type) {
      this.$emit('filter', type)
    },
    clearCompleted() {
      this.$emit('clearCompleted')
    }
  },
  computed: {
    leftCount() {
      return this.list.filter(item => !item.isCompleted).length
    },
    showClear () {
      return this.list.some(item => item.isCompleted)
    }
  },
}
</script>

<style></style>

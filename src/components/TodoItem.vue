<template lang="html">
  <div class="todo-item">
    <div class="todo-item-left">
      <input
      v-model="completed"
      @change="doneEdit"
      type="checkbox"/>
      <div
      v-if="!editing"
      @dblclick="editTodo"
      :class="{ completed: completed}"
      class="todo-item-label">{{ title }}</div>
      <input
      v-else
      v-model="title"
      @blur="doneEdit"
      @keyup.enter="doneEdit"
      @keyup.esc="cancelEdit"
      v-focus
      class="todo-item-edit" type="text"
      />
    </div>
    <div
      @click="removeTodo(index)"
      class="remove-item"
      >
      &times;
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-item',
  props: {
    todo: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    checkAll: {
      type: Boolean,
      required: true,
    }
  },
  data() {
    return {
      'id': this.todo.id,
      'title': this.todo.title,
      'completed': this.todo.completed,
      'editing': this.todo.editing,
      'beforeEditCache': '',
    }
  },
  watch: {
    checkAll() {
      this.completed = this.checkAll ? true : this.todo.completed
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  methods: {
    removeTodo(index) {
      this.$emit('removedTodo', index)
    },
    editTodo() {
      this.beforeEditCache = this.title
      this.editing = true
    },
    doneEdit() {
      if (this.title.trim() == '') {
        this.title = this.beforeEditCache
      }
      this.editing = false
      this.$emit('finishedEdit', {
        'index': this.index,
        'todo': {
          'id': this.id,
          'title': this.title,
          'completed': this.completed,
          'editing': this.editing,
        }
      })
    },
    cancelEdit() {
      this.title = this.beforeEditCache
      this.editing = false
    },
  }
}
</script>

<style lang="css">
</style>

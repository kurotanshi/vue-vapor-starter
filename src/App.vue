<script setup vapor>
import { computed, version } from 'vue'
import { useLocalStorage } from '@vueuse/core'

const tasks = useLocalStorage('tasks', [])
const value = useLocalStorage('value', '')

const remaining = computed(() => {
  return tasks.value.filter((task) => !task.completed).length
})

function handleAdd() {
  tasks.value.push({
    title: value.value,
    completed: false,
  })
  value.value = ''
}

function handleComplete(index, evt) {
  tasks.value[index].completed = (evt.target).checked
}

function handleClearComplete() {
  tasks.value = tasks.value.filter((task) => !task.completed)
}

function handleClearAll() {
  tasks.value = []
}

function handleRemove(idx) {
  tasks.value.splice(idx, 1)
}
</script>

<template>
  <h1>todos</h1>
  <h2>Hello, Vue Vapor!</h2>

  <ul>
    <li
      v-for="({ title, completed }, index) of tasks"
      :key="index"
      :class="{ del: completed }"
    >
      <label>
        <input
          type="checkbox"
          :checked="completed"
          @change="handleComplete(index, $event)"
        />
        {{ title }}
      </label>
      <button @click="handleRemove(index)">x</button>
    </li>
  </ul>

  <p>
    {{ remaining }} item{{ remaining !== 1 ? 's' : '' }} left /
    {{ tasks.length }} item{{ tasks.length !== 1 ? 's' : '' }} in total
  </p>

  <div style="display: flex; gap: 8px">
    <input
      type="text"
      v-model="value"
      @keydown.enter="handleAdd"
      placeholder="What need to be done?"
    />
    <button @click="handleAdd">Add</button>
    <button @click="handleClearComplete">Clear completed</button>
    <button @click="handleClearAll">Clear all</button>
  </div>
  <p>
    Vue Vapor@<code>{{ version }}</code>
  </p>
</template>

<style>
.del {
  text-decoration: line-through;
}
</style>

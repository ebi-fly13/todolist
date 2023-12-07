<script setup lang="ts">
import { computed, ref } from 'vue'

interface Todo {
  name: string
  isFinish: boolean
}

const items = ref<Todo[]>([])

const finishedTodo = computed(() => items.value.filter((todo) => todo.isFinish))
const remainedTodo = computed(() => items.value.filter((todo) => !todo.isFinish))

const newTodoName = ref('')

const addTodo = () => {
  if (newTodoName.value == '') {
    alert('タスク名を入力してください')
    return
  }
  if (items.value.some((todo) => todo.name == newTodoName.value)) {
    alert('同じ名前のタスクが存在します')
    return
  }
  items.value.push({ name: newTodoName.value, isFinish: false })
  newTodoName.value = ''
}

const finishTodo = (todoName: string) => {
  items.value = items.value.map((todo) => {
    if (todo.name == todoName) {
      return { name: todo.name, isFinish: true }
    } else {
      return todo
    }
  })
}
</script>

<template>
  <div>
    <div>TodoList</div>
    <div>完了済みタスク一覧</div>
    <ul>
      <li v-for="item in finishedTodo" :key="item.name">
        <div>名前: {{ item.name }}</div>
      </li>
    </ul>

    <div>未完タスク一覧</div>
    <ul>
      <li v-for="item in remainedTodo" :key="item.name">
        <div>名前: {{ item.name }}</div>
        <button @click="finishTodo(item.name)">完了</button>
      </li>
    </ul>

    <div>
      <label>
        名前
        <input v-model="newTodoName" type="text" />
      </label>
      <button @click="addTodo">追加</button>
    </div>
  </div>
</template>

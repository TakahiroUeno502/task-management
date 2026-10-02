<script setup>
import { ref, onMounted } from 'vue'

const tasks = ref([])
const title = ref('')
const description = ref('')
const editingTaskId = ref(null)

onMounted(async () => {
  const response = await fetch('/api/tasks/')
  tasks.value = await response.json()
})

const addTask = async () => {
  console.log('addTaskが呼ばれました')

  const response = await fetch('/api/tasks/', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      title: title.value,
      description: description.value,
      is_completed: false,
    }),
  })

  const newTask = await response.json()

  tasks.value.unshift(newTask)

  title.value = ''
  description.value = ''
}

const deleteTask = async (task) => {
  await fetch(`/api/tasks/${task.id}/`, {
    method: 'DELETE',
  })

  tasks.value = tasks.value.filter(item => item.id !== task.id)
}

const toggleTask = async (task) => {
  const response = await fetch(`/api/tasks/${task.id}/`, {
    method: 'PATCH',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      is_completed: !task.is_completed,
    }),
  })

  const updatedTask = await response.json()

  task.is_completed = updatedTask.is_completed
}

const startEdit = (task) => {
  editingTaskId.value = task.id
  title.value = task.title
  description.value = task.description
}

const updateTask = async () => {
  const response = await fetch(
    `/api/tasks/${editingTaskId.value}/`,
    {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        title: title.value,
        description: description.value,
      }),
    }
  )

  const updatedTask = await response.json()

  const task = tasks.value.find(
    item => item.id === editingTaskId.value
  )

  task.title = updatedTask.title
  task.description = updatedTask.description

  editingTaskId.value = null
  title.value = ''
  description.value = ''
}

const cancelEdit = () => {
  editingTaskId.value = null
  title.value = ''
  description.value = ''
}

</script>

<template>
  <main>
    <h1>タスク管理</h1>

    <form @submit.prevent="editingTaskId === null ? addTask() : updateTask()">
      <div>
        <label>タイトル</label>
        <input v-model="title" type="text" required>
      </div>

      <div>
        <label>説明</label>
        <textarea v-model="description"></textarea>
      </div>
      <button type="submit">
        {{ editingTaskId === null ? '登録' : '更新' }}
      </button>
      <button
        v-if="editingTaskId !== null"
        type="button"
        @click="cancelEdit"
      >
        キャンセル
      </button>
    </form>

    <div
      v-for="task in tasks"
      :key="task.id"
      class="task-card"
    >
      <h2 :class="{ completed: task.is_completed }">
        {{ task.title }}
      </h2>
      <p>{{ task.description }}</p>
      <p>{{ task.is_completed ? '完了' : '未完了' }}</p>

      <button type="button" @click="toggleTask(task)">
        {{ task.is_completed ? '未完了に戻す' : '完了にする' }}
      </button>
      <button type="button" @click="startEdit(task)">
        編集
      </button>
      <button type="button" @click="deleteTask(task)">
        削除
      </button>
    </div>
  </main>
</template>

<style scoped>
main {
  max-width: 800px;
  margin: 0 auto;
  padding: 30px 20px;
}

form {
  margin-bottom: 30px;
}

form div {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
}

input,
textarea {
  width: 100%;
  box-sizing: border-box;
  padding: 8px;
}

textarea {
  min-height: 80px;
}

.task-card {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 15px;
  margin-bottom: 15px;
}

button {
  padding: 6px 12px;
  margin-right: 8px;
  cursor: pointer;
}

.completed {
  text-decoration: line-through;
}
</style>

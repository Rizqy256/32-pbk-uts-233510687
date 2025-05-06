<template>
  <header class="main-header">
    <div class="header-logo">🧠 MyToDo</div>
  </header>

  <div class="container">
    <h1 class="animated-title">📝 To-Do List</h1>

    <div class="input-section">
      <input v-model="newTask" @keyup.enter="addTask" placeholder="Tambah kegiatan..." />
      <button @click="addTask">Tambah</button>
    </div>

    <hr class="divider" />

    <div class="filter">
      <label>
        <input type="checkbox" v-model="showOnlyUnfinished" />
        Tampilkan hanya yang belum selesai
      </label>
    </div>

    <ul class="task-list">
      <li v-for="(task, index) in filteredTasks" :key="index">
        <input type="checkbox" v-model="task.done" />
        <span :class="{ done: task.done }">{{ task.text }}</span>
        <button @click="removeTask(index)">Hapus</button>
      </li>
    </ul>

    <p v-if="filteredTasks.length === 0" class="empty-msg">
      🌿 Belum ada kegiatan. Yuk mulai hari produktifmu!
    </p>
  </div>
</template>

<script setup>
import { ref, computed, nextTick } from 'vue'

const newTask = ref('')
const tasks = ref([])

const showOnlyUnfinished = ref(false)
const filteredTasks = computed(() =>
  showOnlyUnfinished.value ? tasks.value.filter(task => !task.done) : tasks.value
)

function addTask() {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, done: false })
    newTask.value = ''
    nextTick(() => runConfetti())
  }
}

function removeTask(index) {
  tasks.value.splice(index, 1)
}

function runConfetti() {
  const duration = 600
  const confettiCount = 15
  for (let i = 0; i < confettiCount; i++) {
    const div = document.createElement('div')
    div.className = 'confetti'
    div.style.left = `${Math.random() * 100}%`
    div.style.animationDuration = `${Math.random() * 0.8 + 0.6}s`
    document.body.appendChild(div)
    setTimeout(() => div.remove(), duration)
  }
}
</script>

<style>
/* === HEADER === */
.main-header {
  width: 100%;
  background-color: #0128a8;
  color: #f1f5f9;
  padding: 1rem 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  position: sticky;
  top: 0;
  z-index: 1000;
  margin: 0;
}

.header-logo {
  font-size: 1.5rem;
  font-weight: bold;
}

/* === LAYOUT === */
body {
  font-family: 'Poppins', sans-serif;
  background-color: #0f172a;
  color: #f1f5f9;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}

.container {
  max-width: 600px;
  margin: 3rem auto 0 auto;
  background: #1e293b;
  padding: 2rem;
  border-radius: 12px;
  box-shadow: 0 0 20px rgba(59, 130, 246, 0.2);
  border: 1px solid #3b82f6;
}

/* === TITLE === */
.animated-title {
  animation: slideFadeIn 1s ease-out;
  text-align: center;
  color: #60a5fa;
  margin-bottom: 1.5rem;
}

@keyframes slideFadeIn {
  from {
    opacity: 0;
    transform: translateY(-30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* === INPUT SECTION === */
.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 1rem;
}

.input-section input {
  flex: 1;
  padding: 0.5rem;
  border-radius: 6px;
  border: 1px solid #3b82f6;
  background: #0f172a;
  color: #f1f5f9;
}

.input-section button {
  background-color: #3b82f6;
  color: #fff;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
  transform: scale(1);
  font-family: 'Fredoka', sans-serif;
  font-weight: 500;
  font-size: 1rem;
  letter-spacing: 0.5px;
}

.input-section button:hover {
  background-color: #2563eb;
  transform: scale(1.05);
}

/* === DIVIDER === */
.divider {
  border: none;
  border-top: 1px solid #3b82f6;
  margin: 1rem 0;
  opacity: 0.3;
  border-radius: 99px;
}

/* === FILTER === */
.filter {
  margin-bottom: 1rem;
}

.filter input[type="checkbox"] {
  margin-right: 0.5rem;
  accent-color: #60a5fa;
}

/* === TASK LIST === */
.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-list li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #334155;
  padding: 0.75rem 1rem;
  margin-bottom: 0.5rem;
  border-radius: 6px;
  border-left: 4px solid #3b82f6;
}

.task-list input[type="checkbox"] {
  margin-right: 10px;
  accent-color: #60a5fa;
}

.task-list span {
  flex: 1;
  color: #f1f5f9;
}

.task-list .done {
  text-decoration: line-through;
  color: #94a3b8;
}

.task-list button {
  background-color: transparent;
  border: 1px solid #60a5fa;
  color: #60a5fa;
  padding: 0.25rem 0.75rem;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s ease;
  transform: scale(1);
}

.task-list button:hover {
  background-color: #60a5fa;
  color: #0f172a;
  transform: scale(1.05);
}

/* === EMPTY MESSAGE === */
.empty-msg {
  text-align: center;
  color: #94a3b8;
  margin-top: 1rem;
  font-style: italic;
}

/* === CONFETTI === */
.confetti {
  position: fixed;
  top: 0;
  width: 8px;
  height: 8px;
  background-color: hsl(var(--hue), 70%, 60%);
  animation: confettiFall linear forwards;
  pointer-events: none;
  z-index: 999;
  border-radius: 50%;
  --hue: calc(180 + 60 * random());
}

@keyframes confettiFall {
  0% {
    transform: translateY(-10px) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: translateY(100vh) rotate(720deg);
    opacity: 0;
  }
}
</style>

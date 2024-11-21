<script setup lang="ts">
defineProps<{
  isVisible: boolean
  title: string
}>()

const emit = defineEmits(['close', 'confirm'])

const close = () => emit('close')
const confirm = () => emit('confirm')
</script>

<template>
  <div v-if="isVisible" class="modal-overlay" @click.self="close">
    <div class="modal">
      <header class="header">
        <h3>{{ title }}</h3>
        <button @click="close">Закрыть</button>
      </header>
      <main>
        <slot></slot>
      </main>
      <footer>
        <button @click="confirm">Ок</button>
      </footer>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: white;
  padding: 20px;
  border-radius: 8px;
  min-width: 300px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>

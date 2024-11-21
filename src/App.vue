<script setup lang="ts">
import { ref } from 'vue'
import Modal from '@/components/AppModal.vue'
import FolderTree from '@/components/FolderTree.vue'

const mockFolders = [
  {
    id: 1,
    name: 'Папка 1',
    children: [
      { id: 2, name: 'Папка 1.1', children: [] },
      {
        id: 3,
        name: 'Папка 1.2',
        children: [{ id: 4, name: 'Папка 1.2.1', children: [] }],
      },
    ],
  },
  { id: 5, name: 'Папка 2', children: [] },
]

const isModalOpen = ref(false)
const selectedFolderId = ref<number | null>(null)
const folders = ref(mockFolders)

const openModal = () => (isModalOpen.value = true)
const closeModal = () => (isModalOpen.value = false)

const confirmSelection = () => {
  console.log('Выбранная папка:', selectedFolderId.value)
  closeModal()
}
const onFolderSelect = (id: number) => {
  selectedFolderId.value = id
}
</script>

<template>
  <div class="page">
    <button @click="openModal">Открыть</button>
    <Modal
      :title="'Выберите папку'"
      :is-visible="isModalOpen"
      @close="closeModal"
      @confirm="confirmSelection"
    >
      <FolderTree :folders="folders" :selected-id="selectedFolderId!" @select="onFolderSelect" />
    </Modal>
  </div>
</template>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
</style>

<script setup lang="ts">
interface Folder {
  id: number
  name: string
  children: Folder[]
  isOpen?: boolean
}

const props = defineProps<{
  folders: Folder[]
  selectedId: number
  isOpen?: boolean
}>()

const emit = defineEmits(['select'])

const toggle = (id: number) => {
  const folder = findFolder(props.folders, id)
  if (folder) folder.isOpen = !folder.isOpen
}

const onSelect = (id: number) => {
  emit('select', id)
}

const findFolder = (folders: Folder[], id: number): Folder | null => {
  for (const folder of folders) {
    if (folder.id === id) return folder
    if (folder.children) {
      const found = findFolder(folder.children, id)
      if (found) return found
    }
  }
  return null
}
</script>

<template>
  <ul>
    <li v-for="folder in folders" :key="folder.id">
      <div @click="onSelect(folder.id)" :class="{ selected: selectedId === folder.id }">
        <span @click.stop="toggle(folder.id)">
          <button class="btn" v-if="folder.children && folder.children.length">
            {{ folder.isOpen ? '∨' : '>' }}
          </button>
        </span>
        {{ folder.name }}
      </div>
      <FolderTree
        v-if="folder.children && folder.isOpen"
        :folders="folder.children"
        :selected-id="selectedId"
        @select="onSelect"
      />
    </li>
  </ul>
</template>

<style scoped>
.selected {
  font-weight: bold;
}
.btn {
  cursor: pointer;
}
ul {
  list-style: none;
}
li {
  cursor: pointer;
}
</style>

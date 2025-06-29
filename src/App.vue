<script setup lang="ts">
import { ref } from 'vue'
import { RouterView } from 'vue-router'
import TopNav from './components/TopNav.vue'
import NewHikeModal from './components/NewHikeModal.vue'

const isNewHikeModalOpen = ref(false)

const openNewHikeModal = () => {
  isNewHikeModalOpen.value = true
}

const closeNewHikeModal = () => {
  isNewHikeModalOpen.value = false
}

const handleHikeUploaded = (hikeData: any) => {
  console.log('New hike uploaded:', hikeData)
  // TODO: Handle the uploaded hike data (e.g., refresh hike list, show success message)
}
</script>

<template>
  <TopNav @open-new-hike-modal="openNewHikeModal" />
  <RouterView />
  <NewHikeModal
    :is-open="isNewHikeModalOpen"
    @close="closeNewHikeModal"
    @hike-uploaded="handleHikeUploaded"
  />
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
}
</style>

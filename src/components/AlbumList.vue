<template>
  <div class="album-details-container">
    <div class="content">
      <h2 class="title">Choose Album:</h2>
      <select v-model="selectedAlbum" @change="fetchPhotos" class="album-select">
        <option disabled value="">Select an album</option>
        <option v-for="album in albums" :key="album.id" :value="album.id">{{ album.title }}</option>
      </select>
      <h2 class="title">Photos in Album {{ selectedAlbum }}</h2>
      <div v-if="photos.length" class="photo-grid">
        <div v-for="photo in photos" :key="photo.id" class="photo-item" @click="showPhoto(photo.url)">
          <img :src="photo.thumbnailUrl" class="photo-thumbnail">
          <p class="photo-title">{{ photo.title }}</p>
        </div>
      </div>
      <div v-else class="no-photos">
        <p>No photos available.</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const selectedAlbum = ref(route.params.id || '')
const albums = ref([])
const photos = ref([])

const fetchAlbums = async () => {
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/albums`)
    albums.value = await response.json()
  } catch (error) {
    console.error('Error fetching albums:', error)
  }
}

const fetchPhotos = async () => {
  if (!selectedAlbum.value) return
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/albums/${selectedAlbum.value}/photos`)
    photos.value = await response.json()
  } catch (error) {
    console.error('Error fetching photos:', error)
  }
}

const showPhoto = (url) => {
  window.open(url, '_blank')
}

onMounted(() => {
  fetchAlbums()
  if (selectedAlbum.value) fetchPhotos()
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@400;700&display=swap');

.album-details-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-image: var(--background-url); /* Menggunakan variabel CSS untuk background */
  background-size: cover;
  background-position: center;
  padding: 20px;
  box-sizing: border-box;
}

.content {
  background: rgba(255, 255, 255, 0.95);
  padding: 30px;
  border-radius: 15px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
  max-width: 800px;
  width: 100%;
}

.title {
  font-size: 28px;
  color: #007bff;
  margin-bottom: 20px;
}

.album-select {
  display: block;
  margin: 0 auto 30px auto;
  padding: 12px;
  font-size: 18px;
  border-radius: 8px;
  border: 2px solid #007bff;
  width: 100%;
  max-width: 300px;
  background-color: #f8f9fa;
  outline: none;
  transition: border-color 0.3s, background-color 0.3s;
}

.album-select:focus {
  border-color: #0056b3;
  background-color: #fff;
}

.photo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 20px;
  margin-top: 30px;
}

.photo-item {
  cursor: pointer;
  transition: transform 0.3s;
}

.photo-item:hover {
  transform: translateY(-5px);
}

.photo-thumbnail {
  width: 100%;
  height: 150px;
  object-fit: cover;
  border-radius: 10px;
  border: 2px solid #007bff;
  transition: border-color 0.3s;
}

.photo-item:hover .photo-thumbnail {
  border-color: #0056b3;
}

.photo-title {
  margin-top: 10px;
  font-size: 16px;
  color: #333;
}

.no-photos {
  text-align: center;
  margin-top: 30px;
}

.no-photos p {
  font-size: 18px;
  color: #777;
}
</style>

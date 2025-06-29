<template>
  <nav class="top-nav">
    <div class="nav-container">
      <div class="nav-brand">
        <RouterLink to="/" class="brand-link">
          <svg class="mountain-icon" viewBox="0 0 24 24" fill="currentColor">
            <path d="M12,2L2,19H22L12,2M12,6.7L18,17H6L12,6.7Z" />
          </svg>
          HikeTracker
        </RouterLink>
      </div>
      <div class="nav-links">
        <button @click="openNewHikeModal" class="nav-button">New Hike</button>
        <RouterLink to="/hike-list" class="nav-link">Hike List</RouterLink>
      </div>
    </div>
  </nav>
</template>

<script setup lang="ts">
import { RouterLink } from 'vue-router'

const emit = defineEmits<{
  openNewHikeModal: []
}>()

const openNewHikeModal = () => {
  emit('openNewHikeModal')
}
</script>

<style scoped>
.top-nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  z-index: 1000;
  padding: 0;
  margin: 0;
}

.nav-container {
  padding: 0 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 60px;
}

.nav-brand {
  font-size: 1.5rem;
  font-weight: bold;
}

.brand-link {
  color: #333;
  text-decoration: none;
  transition: color 0.3s ease;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.brand-link:hover {
  color: #667eea;
}

.mountain-icon {
  width: 24px;
  height: 24px;
}

.nav-links {
  display: flex;
  gap: 2rem;
  align-items: center;
}

.nav-link {
  color: #333;
  text-decoration: none;
  font-weight: 500;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  transition: all 0.3s ease;
}

.nav-link:hover {
  background: rgba(102, 126, 234, 0.1);
  color: #667eea;
}

.nav-link.router-link-active {
  background: #667eea;
  color: white;
}

.nav-button {
  background: none;
  border: none;
  color: #333;
  font-weight: 500;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  transition: all 0.3s ease;
  cursor: pointer;
  font-size: 1rem;
  font-family: inherit;
}

.nav-button:hover {
  background: rgba(102, 126, 234, 0.1);
  color: #667eea;
}

@media (max-width: 768px) {
  .nav-container {
    padding: 0 0.5rem;
  }

  .nav-links {
    gap: 1rem;
  }

  .nav-link,
  .nav-button {
    padding: 0.4rem 0.8rem;
    font-size: 0.9rem;
  }
}
</style>

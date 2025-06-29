<template>
  <div class="splash">
    <div class="left-column">
      <div class="content">
        <h1>Track Your Hikes</h1>
        <p class="subtitle">Upload your hiking adventures and plot them on a map</p>

        <div class="button-group">
          <button class="btn btn-primary">
            <svg class="upload-icon" viewBox="0 0 24 24" fill="currentColor">
              <path
                d="M14,2H6A2,2 0 0,0 4,4V20A2,2 0 0,0 6,22H18A2,2 0 0,0 20,20V8L14,2M18,20H6V4H13V9H18V20Z"
              />
            </svg>
            Upload a Hike
          </button>
          <button class="btn btn-secondary">Browse Hikes</button>
        </div>
      </div>
    </div>

    <div class="right-column">
      <div class="image-container">
        <img src="../assets/images/sanjuans.jpg" alt="Mountain Wilson Falls" class="hero-image" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
// Simple splash page component
</script>

<style scoped>
.splash {
  display: flex;
  min-height: 100vh;
  width: 100vw;
  margin: 0;
  padding: 0;
  position: fixed;
  top: 0;
  left: 0;
  background: white;
  color: #333;
  font-family: Arial, sans-serif;
}

.left-column {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
}

.content {
  max-width: 500px;
  text-align: left;
}

h1 {
  font-size: 3.5rem;
  margin-bottom: 1rem;
  text-shadow: none;
  font-weight: bold;
  color: #333;
}

.subtitle {
  font-size: 1.4rem;
  opacity: 0.9;
  margin-bottom: 3rem;
  line-height: 1.5;
}

.button-group {
  display: flex;
  flex-direction: row;
  gap: 1rem;
}

.btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 1rem 2rem;
  border: none;
  border-radius: 8px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  text-decoration: none;
}

.btn-primary {
  background: #4caf50;
  color: white;
}

.btn-primary:hover {
  background: #45a049;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(76, 175, 80, 0.3);
}

.btn-secondary {
  background: white;
  color: #333;
}

.btn-secondary:hover {
  background: #f5f5f5;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(255, 255, 255, 0.3);
}

.upload-icon {
  width: 20px;
  height: 20px;
}

.right-column {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8rem;
}

.image-container {
  width: 100%;
  height: 100%;
  border-radius: 12px;
  overflow: hidden;
}

.hero-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

@media (max-width: 768px) {
  .splash {
    flex-direction: column;
  }

  .left-column,
  .right-column {
    flex: none;
    min-height: 50vh;
  }

  h1 {
    font-size: 2.5rem;
  }

  .subtitle {
    font-size: 1.2rem;
  }
}
</style>

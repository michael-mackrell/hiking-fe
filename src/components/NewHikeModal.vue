<template>
  <Teleport to="body">
    <Transition name="modal">
      <div v-if="isOpen" class="modal-overlay" @click="closeModal">
        <div class="modal-container" @click.stop>
          <div class="modal-header">
            <h2 class="modal-title">Upload New Hike</h2>
            <button @click="closeModal" class="close-button" aria-label="Close modal">×</button>
          </div>

          <div class="modal-content">
            <form @submit.prevent="submitHike" class="hike-form">
              <div class="form-group">
                <label for="hikeName">Hike Name</label>
                <input
                  id="hikeName"
                  v-model="hikeData.name"
                  type="text"
                  required
                  placeholder="Enter hike name"
                  class="form-input"
                />
              </div>

              <div class="form-group">
                <label for="location">Location</label>
                <input
                  id="location"
                  v-model="hikeData.location"
                  type="text"
                  required
                  placeholder="Enter location"
                  class="form-input"
                />
              </div>

              <div class="form-row">
                <div class="form-group">
                  <label for="distance">Distance (miles)</label>
                  <input
                    id="distance"
                    v-model.number="hikeData.distance"
                    type="number"
                    step="0.1"
                    min="0"
                    required
                    placeholder="0.0"
                    class="form-input"
                  />
                </div>

                <div class="form-group">
                  <label for="elevation">Elevation Gain (ft)</label>
                  <input
                    id="elevation"
                    v-model.number="hikeData.elevation"
                    type="number"
                    min="0"
                    required
                    placeholder="0"
                    class="form-input"
                  />
                </div>
              </div>

              <div class="form-group">
                <label for="difficulty">Difficulty Level</label>
                <select id="difficulty" v-model="hikeData.difficulty" required class="form-select">
                  <option value="">Select difficulty</option>
                  <option value="easy">Easy</option>
                  <option value="moderate">Moderate</option>
                  <option value="hard">Hard</option>
                  <option value="expert">Expert</option>
                </select>
              </div>

              <div class="form-group">
                <label for="description">Description</label>
                <textarea
                  id="description"
                  v-model="hikeData.description"
                  rows="3"
                  placeholder="Describe your hike experience..."
                  class="form-textarea"
                ></textarea>
              </div>

              <div class="form-group">
                <label for="hikeDate">Hike Date</label>
                <input
                  id="hikeDate"
                  v-model="hikeData.date"
                  type="date"
                  required
                  class="form-input"
                />
              </div>

              <div class="form-group">
                <label for="photos">Upload Photos</label>
                <input
                  id="photos"
                  type="file"
                  multiple
                  accept="image/*"
                  @change="handlePhotoUpload"
                  class="form-file"
                />
                <div v-if="selectedPhotos.length > 0" class="photo-preview">
                  <div v-for="(photo, index) in selectedPhotos" :key="index" class="photo-item">
                    <img :src="photo.preview" :alt="`Photo ${index + 1}`" />
                    <button type="button" @click="removePhoto(index)" class="remove-photo">
                      ×
                    </button>
                  </div>
                </div>
              </div>

              <div class="form-actions">
                <button type="button" @click="resetForm" class="btn btn-secondary">Reset</button>
                <button type="submit" class="btn btn-primary" :disabled="isSubmitting">
                  {{ isSubmitting ? 'Uploading...' : 'Upload Hike' }}
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup lang="ts">
import { ref, reactive, watch } from 'vue'

interface HikeData {
  name: string
  location: string
  distance: number | null
  elevation: number | null
  difficulty: string
  description: string
  date: string
}

interface Props {
  isOpen: boolean
}

interface Emits {
  (e: 'close'): void
  (e: 'hike-uploaded', hikeData: HikeData): void
}

const props = defineProps<Props>()
const emit = defineEmits<Emits>()

const hikeData = reactive<HikeData>({
  name: '',
  location: '',
  distance: null,
  elevation: null,
  difficulty: '',
  description: '',
  date: '',
})

const selectedPhotos = ref<Array<{ file: File; preview: string }>>([])
const isSubmitting = ref(false)

const closeModal = () => {
  emit('close')
}

const handlePhotoUpload = (event: Event) => {
  const target = event.target as HTMLInputElement
  const files = target.files

  if (files) {
    Array.from(files).forEach((file) => {
      if (file.type.startsWith('image/')) {
        const reader = new FileReader()
        reader.onload = (e) => {
          selectedPhotos.value.push({
            file,
            preview: e.target?.result as string,
          })
        }
        reader.readAsDataURL(file)
      }
    })
  }
}

const removePhoto = (index: number) => {
  selectedPhotos.value.splice(index, 1)
}

const resetForm = () => {
  Object.assign(hikeData, {
    name: '',
    location: '',
    distance: null,
    elevation: null,
    difficulty: '',
    description: '',
    date: '',
  })
  selectedPhotos.value = []
}

const submitHike = async () => {
  isSubmitting.value = true

  try {
    // Create FormData for file upload
    const formData = new FormData()

    // Add hike data
    Object.entries(hikeData).forEach(([key, value]) => {
      if (value !== null && value !== '') {
        formData.append(key, value.toString())
      }
    })

    // Add photos
    selectedPhotos.value.forEach((photo, index) => {
      formData.append(`photos`, photo.file)
    })

    // TODO: Replace with actual API endpoint
    console.log('Submitting hike data:', hikeData)
    console.log('Photos:', selectedPhotos.value)

    // Simulate API call
    await new Promise((resolve) => setTimeout(resolve, 2000))

    // Emit success event
    emit('hike-uploaded', { ...hikeData })

    // Reset form and close modal
    resetForm()
    closeModal()
  } catch (error) {
    console.error('Error uploading hike:', error)
  } finally {
    isSubmitting.value = false
  }
}

// Reset form when modal opens
watch(
  () => props.isOpen,
  (newValue) => {
    if (newValue) {
      resetForm()
    }
  },
)
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  padding: 1rem;
}

.modal-container {
  background: white;
  border-radius: 16px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  width: 100%;
  max-width: 600px;
  max-height: 90vh;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem 2rem;
  border-bottom: 1px solid #e2e8f0;
  background: #f8fafc;
}

.modal-title {
  margin: 0;
  color: #2d3748;
  font-size: 1.5rem;
  font-weight: 700;
}

.close-button {
  background: none;
  border: none;
  font-size: 1.5rem;
  color: #718096;
  cursor: pointer;
  padding: 0.25rem;
  border-radius: 4px;
  transition: all 0.2s ease;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-button:hover {
  background: #e2e8f0;
  color: #2d3748;
}

.modal-content {
  padding: 2rem;
  overflow-y: auto;
  flex: 1;
}

.hike-form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.form-group label {
  font-weight: 600;
  color: #4a5568;
  font-size: 0.9rem;
}

.form-input,
.form-select,
.form-textarea {
  padding: 0.75rem;
  border: 2px solid #e2e8f0;
  border-radius: 8px;
  font-size: 1rem;
  transition: border-color 0.2s ease;
}

.form-input:focus,
.form-select:focus,
.form-textarea:focus {
  outline: none;
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.form-textarea {
  resize: vertical;
  min-height: 80px;
}

.form-file {
  padding: 0.5rem;
  border: 2px dashed #cbd5e0;
  border-radius: 8px;
  background: #f7fafc;
  cursor: pointer;
}

.form-file:hover {
  border-color: #667eea;
  background: #edf2f7;
}

.photo-preview {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(80px, 1fr));
  gap: 0.5rem;
  margin-top: 0.5rem;
}

.photo-item {
  position: relative;
  border-radius: 8px;
  overflow: hidden;
}

.photo-item img {
  width: 100%;
  height: 80px;
  object-fit: cover;
}

.remove-photo {
  position: absolute;
  top: 4px;
  right: 4px;
  background: rgba(255, 0, 0, 0.8);
  color: white;
  border: none;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
}

.remove-photo:hover {
  background: rgba(255, 0, 0, 1);
}

.form-actions {
  display: flex;
  gap: 1rem;
  justify-content: flex-end;
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid #e2e8f0;
}

.btn {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 1rem;
}

.btn-primary {
  background: #667eea;
  color: white;
}

.btn-primary:hover:not(:disabled) {
  background: #5a67d8;
  transform: translateY(-1px);
}

.btn-primary:disabled {
  background: #a0aec0;
  cursor: not-allowed;
  transform: none;
}

.btn-secondary {
  background: #e2e8f0;
  color: #4a5568;
}

.btn-secondary:hover {
  background: #cbd5e0;
}

/* Modal transitions */
.modal-enter-active,
.modal-leave-active {
  transition: all 0.3s ease;
}

.modal-enter-from {
  opacity: 0;
  transform: scale(0.9);
}

.modal-leave-to {
  opacity: 0;
  transform: scale(0.9);
}

@media (max-width: 768px) {
  .modal-overlay {
    padding: 0.5rem;
  }

  .modal-container {
    max-height: 95vh;
  }

  .modal-header {
    padding: 1rem 1.5rem;
  }

  .modal-content {
    padding: 1.5rem;
  }

  .form-row {
    grid-template-columns: 1fr;
  }

  .form-actions {
    flex-direction: column;
  }

  .photo-preview {
    grid-template-columns: repeat(auto-fill, minmax(60px, 1fr));
  }

  .photo-item img {
    height: 60px;
  }
}
</style>

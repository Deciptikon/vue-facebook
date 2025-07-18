<template>
  <div>
    <h1>Фотоальбом</h1>

    <div class="photo-grid">
      <div v-for="n in photoCount" :key="n" class="photo-item" @click="openModal(n)">
        <img :src="getThumbnailUrl(n)" :alt="`Фото ${n}`" />
        <p>Фото {{ n }}</p>
      </div>
    </div>

    <!-- Модальное окно для просмотра фото -->
    <div v-if="isModalOpen" class="modal" @click.self="closeModal">
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>

        <div class="main-photo">
          <img :src="getFullsizeUrl(currentPhoto)" :alt="`Фото ${currentPhoto}`" />
        </div>

        <div class="photo-info">
          <p>Фото {{ currentPhoto }} из {{ photoCount }}</p>
        </div>

        <div class="navigation">
          <button @click="prevPhoto" :disabled="currentPhoto === 1" class="nav-btn">
            &lt; Назад
          </button>
          <button @click="nextPhoto" :disabled="currentPhoto === photoCount" class="nav-btn">
            Вперед &gt;
          </button>
        </div>
      </div>
    </div>

    <router-link :to="{ name: 'home' }">На главную</router-link>
  </div>
</template>

<script>
export default {
  name: 'PhotoalbumView',
  data() {
    return {
      photoCount: 5, // Количество фотографий
      isModalOpen: false,
      currentPhoto: 1,
    }
  },
  methods: {
    getThumbnailUrl(n) {
      //return `/images/photoalbum/thumb/photo${n}.jpeg`
      return `./images/photoalbum/photo${n}.jpeg`
    },
    getFullsizeUrl(n) {
      //return `/images/photoalbum/full/photo${n}.jpeg`
      return `./images/photoalbum/photo${n}.jpeg`
    },
    openModal(n) {
      this.currentPhoto = n
      this.isModalOpen = true
      // Блокируем прокрутку страницы при открытом модальном окне
      document.body.style.overflow = 'hidden'
    },
    closeModal() {
      this.isModalOpen = false
      // Восстанавливаем прокрутку страницы
      document.body.style.overflow = ''
    },
    nextPhoto() {
      if (this.currentPhoto < this.photoCount) {
        this.currentPhoto++
      }
    },
    prevPhoto() {
      if (this.currentPhoto > 1) {
        this.currentPhoto--
      }
    },
  },
}
</script>

<style scoped>
/* Стили для сетки фотографий (оставляем предыдущие) */
.photo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
  margin: 30px 0;
}

.photo-item {
  cursor: pointer;
  transition: transform 0.3s ease;
}

.photo-item:hover {
  transform: scale(1.05);
}

/* Стили модального окна */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  max-width: 90%;
  max-height: 90%;
  position: relative;
  display: flex;
  flex-direction: column;
}

.close {
  position: absolute;
  top: 10px;
  right: 15px;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
  color: #333;
}

.close:hover {
  color: #f00;
}

.main-photo {
  flex-grow: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px 0;
}

.main-photo img {
  max-width: 100%;
  max-height: 70vh;
  object-fit: contain;
}

.photo-info {
  text-align: center;
  margin-bottom: 15px;
  font-size: 18px;
}

.navigation {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}

.nav-btn {
  padding: 8px 16px;
  font-size: 16px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.nav-btn:hover:not(:disabled) {
  background-color: #358f65;
}

.nav-btn:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}
</style>

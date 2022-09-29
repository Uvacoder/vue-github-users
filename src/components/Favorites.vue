<script setup>
const props = defineProps({
  favoritesList: {
    type: Array,
    required: true
  }
})

const { favoritesList } = props
</script>

<template>
  <div>
    <p v-if="favoritesList.length" class="favorites__title">Favorites</p>
    <div class="favorites">
      <div>
        <div class="favorites__container" v-for="user in favoritesList" :key="user.login">
          <div class="favorite" @click="showFavorite(user)">
            <img :src="user.avatar_url" alt="user.login" />
          </div>
          <span>@{{ user.login }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.favorites__title {
  position: absolute;
  margin: 1em;
  font-weight: 600;
}

.favorites {
  margin-top: 2em;
  padding: 1em;
  display: flex;
  gap: 1em;
  grid-column: 1 / 4;
  flex-wrap: wrap;
  align-content: flex-start;
}

.favorites__container {
  height: fit-content;
}

.favorite {
  width: fit-content;
  height: 60px;
  background-color: #1f2a48;
  border-radius: 8px;
  overflow: hidden;
  cursor: pointer;
}

.favorites__container:hover span {
  opacity: 1;
}

.favorite img {
  height: 100%;
}

.favorites__container span {
  opacity: 0;
  margin-top: 10px;
  padding: 6px 10px;
  font-size: 14px;
  position: absolute;
  z-index: 100;
  background-color: #0b0f18;
  border-radius: 6px;
  transition: opacity 0.3s ease;
}
</style>

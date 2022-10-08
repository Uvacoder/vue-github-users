<script setup>
import { onMounted, ref, watch } from 'vue'
import Favorites from './components/Favorites.vue'
import ResultCard from './components/ResultCard/index.vue'
import SearchIcon from './components/Icons/SearchIcon.vue'
import vAutoAnimate from '@formkit/auto-animate'
import { computed } from '@vue/reactivity'

const search = ref(null)
const currentUser = ref(null)
const result = ref(null)

const favorites = ref(new Map())

const favoritesArray = computed(() => {
  return Array.from(favorites.value.values())
})

const isFavorite = computed(() => {
  return favorites.value.has(currentUser.value.login)
})

const setUser = user => {
  search.value = user.login
  doSearch()
}

const error = ref(null)

const doSearch = async () => {
  const API_URL = 'https://api.github.com/users/'

  currentUser.value = null
  error.value = null

  result.value = true
  try {
    if (search.value) {
      const response = await fetch(API_URL + search.value)
      const data = await response.json()

      if (!response.ok) throw new Error('Not Found')
      currentUser.value = data
    } else result.value = null
  } catch (err) {
    error.value = err.message
  }
  search.value = null
}

watch(favoritesArray, () => {
  localStorage.setItem('favorites', JSON.stringify(favoritesArray.value))
})

const toggleFavorite = () => {
  if (!isFavorite.value) {
    favorites.value.set(currentUser.value.login, currentUser.value)
    return
  }
  favorites.value.delete(currentUser.value.login)
}

onMounted(() => {
  const savedFavorites = JSON.parse(localStorage.getItem('favorites'))
  if (savedFavorites?.length) {
    const newFavorites = new Map(savedFavorites.map(favorite => [favorite.login, favorite]))
    favorites.value = newFavorites
  }
})
</script>

<template>
  <!-- Favorites -->
  <Favorites :favoritesList="favoritesArray" @showUser="setUser" />

  <div class="main">
    <h3>devfinder</h3>

    <!-- Search Input -->
    <div class="search">
      <form action="" class="form" @submit.prevent="doSearch">
        <div class="input__container">
          <SearchIcon class="search-icon" />
          <input
            type="text"
            name="name"
            id="search__input"
            placeholder="Search Github Username..."
            v-model="search"
          />
          <button type="submit" class="btn">Search</button>
        </div>
      </form>
    </div>

    <!-- Result -->
    <div v-if="result" class="info">
      <ResultCard
        v-auto-animate
        v-if="currentUser"
        :user="currentUser"
        :isFavorite="isFavorite"
        @toggleFavorite="toggleFavorite"
      />
      <!-- Error -->
      <div v-if="error" class="error">
        <p> {{ error }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.main {
  grid-column: 2 / 3;
  grid-row: 2 / 3;
}

.search,
.info {
  background-color: #1f2a48;
  padding: 10px;
  border-radius: 10px;
  min-width: 500px;
}

.search {
  margin: 12px 0;
}

.search-icon {
  color: #007afe;
}
.btn {
  border-radius: 8px;
  background-color: #007afe;
  cursor: pointer;
}

.btn:hover {
  background-color: #0263ca;
}

.input__container {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 8px;
}
</style>

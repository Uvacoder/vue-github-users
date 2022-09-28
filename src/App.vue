<script setup>
import { reactive, ref } from 'vue'
import Favorites from './components/Favorites.vue'
import ResultCard from './components/ResultCard/index.vue'
import SearchIcon from './components/Icons/SearchIcon.vue'

const favorites = reactive(new Map())

const search = ref(null)
const currentUser = ref(null)

const error = ref(null)

const doSearch = async () => {
  const API_URL = 'https://api.github.com/users/'

  currentUser.value = null
  error.value = null

  try {
    if (search.value) {
      const response = await fetch(API_URL + search.value)
      const data = await response.json()

      if (!response.ok) throw new Error('Not Found')
      currentUser.value = data
      search.value = null
    }
  } catch (err) {
    error.value = err.message
  }
}
</script>

<template>
  <Favorites :favoritesList="favorites" />
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
    <ResultCard v-if="currentUser" :user="currentUser" class="info" :error="error" />
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

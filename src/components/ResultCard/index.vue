<script setup>
import UserStats from './UserStats.vue'
import UserContact from './UserContact.vue'
import HeartIcon from '../Icons/HeartIcon.vue'

const props = defineProps({
  user: {
    type: Object,
    required: true
  }
})

const { user } = props

user.joined = new Date(user.created_at).toLocaleDateString('en-UK', {
  year: 'numeric',
  month: 'short',
  day: 'numeric'
})
</script>

<template>
  <div>
    <div class="info">
      <div class="info__user">
        <!-- Image -->
        <img class="user__img" :src="user.avatar_url" :alt="user.login" />
        <div class="user__title">
          <div>
            <span>
              <h3 class="name">{{ user.name }}</h3>
              <a :href="user.html_url" rel="noopener" target="_blank" class="username"> @{{ user.login }} </a>
            </span>
            <p class="joined">Joined {{ user.joined }}</p>
          </div>
          <p class="bio">{{ user.bio || 'This profile has no bio' }}</p>
        </div>

        <UserStats :user="user" />
        <button class="toggle-favorite"> <HeartIcon /> </button>
        <UserContact :user="user" />
      </div>
    </div>
  </div>
</template>

<style>
.error {
  padding: 1em;
  font-weight: 600;
  background-color: #c52747;
  border-radius: 8px;
}

.info__user {
  width: 100%;
  padding: 24px;
  display: grid;
  gap: 1.2em;
  grid-template-columns: min-content repeat(2, 1fr);
  grid-template-rows: repeat(3, max-content);
}

.user__img {
  width: 100px;
  border-radius: 50%;
}

.user__title {
  grid-column: 2 / 4;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.user__title div {
  display: flex;
  justify-content: space-between;
}

.user__title p,
.joined {
  font-size: 0.8em;
}

.name {
  max-width: 250px;
}

.joined {
  text-align: right;
}

.username {
  color: #0263ca;
  margin-bottom: 10px;
  font-size: 0.8em;
}

.bio {
  color: #677189;
  max-width: 420px;
}

.toggle-favorite {
  display: flex;
  color: #1f2a48;
  font-size: 18px;
  padding: 8px 10px;
  place-self: end start;
  border-radius: 5px;
  cursor: pointer;
}
</style>

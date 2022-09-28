<script setup>
const props = defineProps({
  user: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div class="user__stats">
    <div>
      <h4>Repos</h4>
      <span>{{ props.user.public_repos }}</span>
    </div>
    <div>
      <h4>Followers</h4>
      <span>{{ props.user.followers }}</span>
    </div>
    <div>
      <h4>Following</h4>
      <span>{{ props.user.following }}</span>
    </div>
  </div>
</template>

<style>
.user__stats {
  background-color: #141c2f;
  display: flex;
  justify-content: space-around;
  padding: 12px;
  grid-column: 2 / 4;
  border-radius: 8px;
}
</style>

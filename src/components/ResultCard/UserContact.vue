<script setup>
import PinIcon from '../Icons/PinIcon.vue'
import TwitterIcon from '../Icons/TwitterIcon.vue'
import LinkIcon from '../Icons/LinkIcon.vue'
import BuildingIcon from '../Icons/BuildingIcon.vue'
const props = defineProps({
  user: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div class="user__contact">
    <span>
      <PinIcon />
      <p class="location">{{ props.user.location || 'Not available' }}</p>
    </span>
    <span>
      <TwitterIcon />
      <a :href="'https://twitter.com/' + (props.user.twitter_username || '')" rel="noopener" target="_blank">
        {{ props.user.twitter_username || 'Not available' }}
      </a>
    </span>
    <span>
      <LinkIcon />
      <a class="blog" :href="props.user.blog" rel="noopener" target="_blank">
        {{ props.user.blog || 'Not available' }}
      </a>
    </span>
    <span>
      <BuildingIcon />
      <p class="company">{{ props.user.company || 'Not available' }}</p>
    </span>
  </div>
</template>

<style>
.user__contact {
  font-size: 14px;
  grid-column: 2 / 4;
  display: grid;
  grid-template-columns: repeat(2, minmax(170px, max-content));
  grid-auto-rows: 20px;
  gap: 1.4em 8%;
}

.user__contact span {
  display: flex;
  align-items: center;
  gap: 10px;
}

.company {
  max-width: 170px;
}

.blog {
  max-width: 160px;
  word-wrap: break-word;
}
</style>

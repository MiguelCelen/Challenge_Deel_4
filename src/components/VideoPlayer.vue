<script setup>
const props = defineProps({
  src: String,
  username: String,
  caption: String
})

const emit = defineEmits(['open-chat'])
</script>

<template>
  <div class="video-player">
    <video :src="props.src" autoplay muted loop playsinline></video>
    <div class="buttons">
      <button @click="emit('open-chat')">💬</button>
    </div>
    <div class="details">
      <h3>@{{ props.username }}</h3>
      <p>{{ props.caption }}</p>
    </div>
  </div>
</template>

<style scoped>
.video-player {
  position: relative;
  width: 100%;
  max-width: 460px;
  height: 100vh;
  margin: 0 auto;
  scroll-snap-align: start;
}

video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.buttons {
  position: absolute;
  right: 14px;
  bottom: 120px;
}

.buttons button {
  background: none;
  border: none;
  font-size: 2rem;
  cursor: pointer;
}

.details {
  position: absolute;
  left: 14px;
  bottom: 28px;
  right: 90px;
  text-shadow: 0 1px 4px rgba(0, 0, 0, 0.7);
}

.details h3 {
  font-size: 1rem;
}

.details p {
  font-size: 0.9rem;
  margin-top: 4px;
}
</style>

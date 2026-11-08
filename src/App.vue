<script setup>
import { reactive, ref } from 'vue'
import VideoPlayer from './components/VideoPlayer.vue'
import Chat from './components/Chat.vue'

const videos = reactive([
  {
    id: 1,
    src: 'https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4',
    username: 'bloemenfan',
    caption: 'Kijk hoe deze bloem opengaat 🌸'
  },
  {
    id: 2,
    src: 'https://interactive-examples.mdn.mozilla.net/media/cc0-videos/friday.mp4',
    username: 'weekendmood',
    caption: 'Het is eindelijk vrijdag! 🎉'
  }
])

const chatOpen = ref(false)
</script>

<template>
  <div id="feed">
    <VideoPlayer
      v-for="video in videos"
      :key="video.id"
      :src="video.src"
      :username="video.username"
      :caption="video.caption"
      @open-chat="chatOpen = true"
    />
  </div>

  <Chat v-if="chatOpen" @close="chatOpen = false" />
</template>

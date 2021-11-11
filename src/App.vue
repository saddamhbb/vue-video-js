<template>
    <video 
      ref="videoPlayer" 
      controls
      class="video-js" 
      :options="options"
    >
    </video>
</template>

<script>
import videojs from 'video.js'
import 'video.js/dist/video-js.css'
import videoHewan from './assets/VideoHewan.mp4'

export default {
  name: "App",
  data() {
    return {
      player: null,
      options: {
        sources: [
          {
            src: videoHewan,
            type: 'video/mp4'
          }
        ]
      }
    }
  },
  mounted() {
    this.player = videojs(this.$refs.videoPlayer, this.options, function onPlayerReady() {
      const videoPlayer = this;
      let currentTime = 0;

      videoPlayer.on("seeking", function() {
        if (currentTime < videoPlayer.currentTime()) {
          videoPlayer.currentTime(currentTime);
        }
      });

      videoPlayer.on("seeked", function() {
        if (currentTime < videoPlayer.currentTime()) {
          videoPlayer.currentTime(currentTime);
        }
      });

      videoPlayer.setInterval(() => {
        if (!videoPlayer.paused()) {
          currentTime = videoPlayer.currentTime()
        }
      }, 1000);
    })
  },
  beforeDestroy() {
    if (this.player) {
      this.player.dispose()
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.video-js {
  width: 100%;
}
</style>

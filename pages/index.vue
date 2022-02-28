<template>
  <div>
    <h1>Custom video player with nuxtjs</h1>
    <video
      ref="video"
      :src="require('~/assets/videos/gone.mp4')"
      id="video"
      class="screen"
      :poster="require('~/assets/img/poster.png')"
      @click="toggleVideoStatus"
    ></video>
    <div class="controls">
      <div class="btn-control-group">
        <v-btn
          @click="toggleVideoStatus"
          icon
          :color="!isPlaying ? 'white' : 'success'"
        >
          <v-icon>{{ !isPlaying ? "mdi-play" : "mdi-pause" }}</v-icon>
        </v-btn>
        <v-btn @click="stopVideo" icon color="error">
          <v-icon>mdi-stop</v-icon>
        </v-btn>
      </div>
      <!-- <button class="btn" id="play">
          <i class="fa fa-play fa-2x"></i>
        </button>
        <button class="btn" id="stop">
          <i class="fa fa-stop fa-2x"></i>
        </button> -->
      <!-- <input
          type="range"
          id="progress"
          class="progress"
          min="0"
          max="100"
          step="0.1"
          value="0"
        /> -->
      <v-slider
        min="0"
        max="100"
        dense
        step="0.1"
        hide-details
        v-model="videoBar.val"
        :track-color="videoBar.color"
        @change="setVideoProgress"
      ></v-slider>
      <span class="timestamp" id="timestamp">{{ timestamp }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isPlaying: false,
      videoBar: { val: 0, color: "green lighten-1" },
      timestamp: "00:00",
    };
  },
  methods: {
    toggleVideoStatus() {
      if (this.$refs.video.paused) {
        this.$refs.video.play();
        this.isPlaying = true;
      } else {
        this.$refs.video.pause();
        this.isPlaying = false;
      }
    },
    stopVideo() {
      this.$refs.video.currentTime = 0;
      this.$refs.video.pause();
      this.isPlaying = false;
    },
    updateTime() {
      this.videoBar.val =
        (this.$refs.video.currentTime / this.$refs.video.duration) * 100;

      // setting time minutes
      let mins = Math.floor(this.$refs.video.currentTime / 60);
      if (mins < 10) {
        mins = "0" + String(mins);
      }

      // setting time seconds
      let secs = Math.floor(this.$refs.video.currentTime % 60);
      if (secs < 10) {
        secs = "0" + String(secs);
      }
      this.timestamp = `${mins}:${secs}`;

      if (this.$refs.video.currentTime == this.$refs.video.duration) {
        this.isPlaying = false;
        this.videoBar.val = 0;
      }
    },
    setVideoProgress() {
      this.$refs.video.currentTime =
        (+this.videoBar.val * this.$refs.video.duration) / 100;
      console.log("test progress");
    },
  },
  mounted() {
    this.$refs.video.addEventListener("timeupdate", this.updateTime);
  },
};
</script>

<style>
h1 {
  color: #0ba37d;
  text-shadow: 5px 5px 5px rgba(0, 0, 0, 0.5);
  letter-spacing: 3px;
  word-spacing: 10px;
  margin-bottom: 20px;
  font-size: 7vmin;
  text-align: center;
}
.screen {
  cursor: pointer;
  width: min(900px, 100%);
  min-width: 900px;
  background: #000 !important;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  max-height: 600px;
  min-height: 600px;
  max-width: 900px;
}
.controls {
  width: min(900px, 100%);
  min-width: 900px;
  background: #000;
  color: #fff;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 12px;
  gap: 10px;
  margin-top: -28px;
}

.timestamp {
  letter-spacing: 1px;
}
</style>

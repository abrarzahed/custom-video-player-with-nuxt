<template>
  <v-app>
    <!-- <v-main> -->
    <div class="video-wrapper">
      <Nuxt />
    </div>
    <!-- </v-main> -->
  </v-app>
</template>

<script>
export default {};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Lilita+One&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Lilita One", cursive;
}
.video-wrapper {
  min-height: 100vh;
  display: grid;
  align-items: center;
  justify-content: center;
}
</style>

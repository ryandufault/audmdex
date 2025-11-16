<template>
  <div class="about-wrapper" @click="close">
    <div class="about-container" @click.stop>
        <p class="about-header">about audmdex</p>
        <p><strong>audmdex</strong> lets you quickly inspect audio metadata right in your browser. <br><br>
        file uploading is <strong>client-sided</strong>, so is the processing. no files are uploaded or sent to
        any server, everything is read directly into your computer's temporary memory, and disappears 
        the moment you close or refresh the page.<br><br>
        audmdex uses the library <strong>music-metadata-browser</strong><a href="https://github.com/Borewit/music-metadata-browser" target="_blank">↗</a> to extract metadata from files. it's fast, reliable, and built for safe in-browser analysis.
        no ads, no tracking, no nonsense. <br><br>just a simple tool that works anywhere, <strong>whenever you need it.</strong>
        </p>
        <br><br><br>
        <p><a href="https://github.com/ryandufault" target="_blank">made with care by Ryan Dufault</a></p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'About',
  
  data() {
    return {
      
    }
  }
}
</script>

<script setup>
const emit = defineEmits(['close'])

function close() {
  emit('close')
}
</script>

<style scoped>
.about-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  backdrop-filter: brightness(60%);
  display: flex;
  justify-content: center;
  align-items: center;
}

.about-header {
    color: #dfdfe4 !important;
    font-weight: normal;
    margin-bottom: 2rem;
    font-size: 1.5rem !important;
}

.about-container {
  width: 45vw;
  height: 65vh;
  background-color: rgb(0, 0, 0, 0.9);
  border-radius: 25px;
  display: flex;
  justify-self: center;
  align-self: center;
  padding: 2vw;
  flex-direction: column;
  align-items: flex-start;
  filter: drop-shadow(0px 2px 14px rgb(125,125,125, 0.1));
}

.about-container p {
  color: #a1a1a4;
  font-size: 1rem;
  transition: color 0.3s ease;
}

a:hover {
  color: #dfdfe4;
}

a {
  color: #c3c3c5;
  font-size: 1rem;
  vertical-align: 15%;
  transition: color 0.3s ease;
}

strong {
  color: #dfdfe4;
  font-weight: normal;
}
</style>
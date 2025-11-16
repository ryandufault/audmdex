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
        <p><a href="https://github.com/ryandufault" target="_blank">made with care by ryan dufault</a></p>
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
  backdrop-filter: brightness(50%);
  display: flex;
  justify-content: center;
  align-items: center;
}

.about-header {
    color: #dfdfe4 !important;
    font-weight: normal;
    margin-bottom: 2vw;
    font-size: 1.5vw !important;
}

.about-container {
  width: 45vw;
  height: 55vh;
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
  font-size: 1vw;
  transition: color 0.3s ease;
}

a:hover {
  color: #dfdfe4;
}

a {
  color: #c3c3c5;
  font-size: 1vw;
  vertical-align: 15%;
  transition: color 0.3s ease;
}

strong {
  color: #dfdfe4;
  font-weight: normal;
}


/* tablet */
@media (max-width: 1320px) {
  .about-container {
    width: 80vw;
    height: 70vh;
    padding: 1.5vw;
  }

  .about-header {
    margin-top: 1.5vw;
    font-size: 3.2vw !important;
    margin-bottom: 1.5vw;
  }

  .about-container p {
    font-size: 2.1vw;
  }

  a {
    font-size: 2.1vw;
  }
}

/* mobile */
@media (max-width: 768px) {
  .about-wrapper {
    align-items: center;
  }

  .about-container {
    width: 90%;
    height: auto;
    max-height: 80vh;
    padding: 20px;
    border-radius: 15px;
  }

  .about-header {
    font-size: 18px !important;
    margin-bottom: 15px;
  }

  .about-container p {
    font-size: 13px;
    line-height: 1.5;
  }

  a {
    font-size: 13px;
  }
}

/* sm mobile */
@media (max-width: 420px) {
  .about-container {
    width: 95%;
    padding: 15px;
  }

  .about-header {
    font-size: 16px !important;
  }

  .about-container p {
    font-size: 12px;
  }

  a {
    font-size: 12px;
  }
}
</style>
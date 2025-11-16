<script setup>
import { ref } from 'vue'
import FileUpload from './components/FileUpload.vue'
import About from './components/About.vue'
import DisplayMetadata from './components/DisplayMetadata.vue'

const showAbout = ref(false)
</script>

<template>
  <div class="app-wrapper">
    <div class="app-container">
      <div class="header">
        <h1>audmdex</h1>
        <h2>your go-to audio metadata extractor.</h2>
      </div>
      <FileUpload @metadata-extracted="handleMetadata"/>
      <!-- open seulement quand il y a des metadata -->
      <DisplayMetadata v-if="metadataInfo" :metadata="metadataInfo"/>
      <footer>
        <a class="footerlink" href="https://ryandufault.com" target="_blank">made by ryan dufault</a>
        <!-- <a href="https://github.com/ryandufault" target="_blank">Github</a>-->
        <p class="footerlink" @click="showAbout = true">about</p>

      </footer>
    </div>
    <!-- open quand true-->
    <About v-if="showAbout" @close="showAbout = false" />
  </div>
</template>

<script>
import FileUpload from './components/FileUpload.vue'
import DisplayMetadata from './components/DisplayMetadata.vue'

export default {
  components: {
    FileUpload,
    DisplayMetadata
  },
  
  data() {
    return {
      metadataInfo: null // store la metadata dynamiquement
    }
  },
  
  methods: {
    handleMetadata(data) {
      this.metadataInfo = data; // store la metadata reçue
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  background-color: #090909;
}

#app {
  width: 100%;
  height: 100vh;
}
</style>

<style scoped>
.app-wrapper {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #090909;
  font-family: "Lucida Console", "Courier New", monospace;
  color: #f1f1f4;
}

.app-container {
  width: 55vw;
  height: 75vh;
  display: flex;
  flex-direction: column;
}

.header {
  margin-bottom: 2vw;
  text-align: left;
}

h1 {
  color: #f1f1f4;
  font-size: 2.5vw;
  margin-bottom: 0.5vw;
}

h2 {
  color: #a1a1a4;
  font-size: 1vw;
  font-weight: normal;
}

footer {
  margin-top: 2vw;
  padding-bottom: 12.5vh;
  display: flex;
  justify-content: space-between;
}

footer a {
  color: #a1a1a4;
  font-size: 0.85vw;
  font-weight: normal;
  text-decoration: none;
  transition: color 0.3s ease;
}

.footerlink:hover {
  color: #c3c3c5;
}

footer p {
  color: #a1a1a4;
  font-size: 0.85vw;
  font-weight: normal;
  text-decoration: none;
  cursor: pointer;
  transition: color 0.3s ease;
}
</style>
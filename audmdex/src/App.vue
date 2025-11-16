<script setup>
import { ref } from 'vue'
import FileUpload from './components/FileUpload.vue'
import About from './components/About.vue'

const showAbout = ref(false)
</script>

<template>
  <div class="app-wrapper">
    <div class="app-container">
      <div class="header">
        <h1>audmdex</h1>
        <h2>your go-to audio metadata extractor.</h2>
      </div>
      <FileUpload/>
      <footer>
        <a class="footerlink" href="https://ryandufault.com" target="_blank">made by Ryan Dufault</a>
        <!-- <a href="https://github.com/ryandufault" target="_blank">Github</a>-->
        <p class="footerlink" @click="showAbout = true">about</p>

      </footer>
    </div>
    <!-- open quand true-->
    <About v-if="showAbout" @close="showAbout = false" />
  </div>
</template>

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
  margin-bottom: 2rem;
  text-align: left;
}

h1 {
  color: #f1f1f4;
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
}

h2 {
  color: #a1a1a4;
  font-size: 1rem;
  font-weight: normal;
}

footer {
  margin-top: 2rem;
  display: flex;
  justify-content: space-between;
}

footer a {
  color: #a1a1a4;
  font-size: 0.85rem;
  font-weight: normal;
  text-decoration: none;
  transition: color 0.3s ease;
}

.footerlink:hover {
  color: #c3c3c5;
}

footer p {
  color: #a1a1a4;
  font-size: 0.85rem;
  font-weight: normal;
  text-decoration: none;
  cursor: pointer;
  transition: color 0.3s ease;
}
</style>
<script setup>
import FileUpload from './components/FileUpload.vue'
</script>

<template>
  <div class="app-wrapper">
    <div class="app-container">
      <div class="header">
        <h1>audmdex</h1>
        <h2>your go-to audio metadata extractor.</h2>
      </div>
      <FileUpload/>
    </div>
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
</style>
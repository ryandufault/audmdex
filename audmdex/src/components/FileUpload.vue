<template>
  <div class="file-container" @click="handleClick">
    <input 
      ref="fileInput"
      type="file"
      accept="audio/*"
      @change="handleFileSelect"
      style="display: none;"
    />
    <p>click me to upload a file!</p>
  </div>
</template>

<script>
export default {
  name: 'FileUpload',
  
  methods: {
    handleClick() {
      this.$refs.fileInput.click();
    },
    
    // si l'user choisis un fichier
    handleFileSelect(event) {
      const file = event.target.files[0];
      
      if (file) {
        console.log('Fichier selected');
        this.processFile(file);
      }
    },
    
    // processing; console logs pour l'instant
    processFile(file) {
      console.log('details du fichier :');
      console.log('Name:', file.name);
      console.log('Size:', file.size, 'bytes');
      console.log('Size (MB):', (file.size / 1024 / 1024).toFixed(2), 'MB');
      console.log('Type:', file.type);
      console.log('Last Modified:', new Date(file.lastModified));
      console.log('Full File Object:', file);
    }
  }
}
</script>

<style scoped>
.file-container {
  width: 100%;
  flex: 1;
  background-color: rgba(0, 0, 0, 0);
  border: 6px dotted #cfcfcf;
  border-radius: 25px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: border-color 0.3s ease;
}

.file-container:hover {
  border-color: #f1f1f4;
}

.file-container p {
  color: #a1a1a4;
  font-size: 1.2rem;
}
</style>
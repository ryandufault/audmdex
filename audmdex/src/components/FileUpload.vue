<template>
  <div class="file-container" @click="handleClick">
    <input 
      ref="fileInput"
      type="file"
      accept="audio/*"
      @change="handleFileSelect"
      style="display: none;"
    />
    <p>{{ statusMessage }}</p>
  </div>
</template>

<script>
import * as musicMetadata from 'music-metadata-browser';
import { Buffer } from 'buffer';

window.Buffer = Buffer;

export default {
  name: 'FileUpload',
  
  data() {
    return {
      statusMessage: 'click me to upload a file!' // par défaut, changera dynamiquement
    }
  },
  
  methods: {
    handleClick() {
      this.$refs.fileInput.click();
    },
    
    handleFileSelect(event) {
      const file = event.target.files[0];
      
      if (file) {
        console.log('Fichier selected');
        this.processFile(file);
      }
    },
    
    async processFile(file) {
      console.log(' ---- DETAILS DU FICHIER ----- ');
      console.log('Name:', file.name);
      console.log('Size (MB):', (file.size / 1024 / 1024).toFixed(2), 'MB');
      console.log('Type:', file.type);
      
      // update le status
      this.statusMessage = 'extracting metadata...';
      
      try {
        // extrait les données avec la librairie
        const metadata = await musicMetadata.parseBlob(file);
        
        console.log(' ---- METADATA EXTRACTED ----- ');
        console.log('Title:', metadata.common.title || 'X');
        console.log('Artist:', metadata.common.artist || 'X');
        console.log('Album:', metadata.common.album || 'X');
        console.log('Year:', metadata.common.year || 'X');
        console.log('Genre:', metadata.common.genre?.join(', ') || 'X');
        
        // détails techniques
        console.log(' ---- INFOS TECHNIQUES ----- ');
        console.log('Duration:', metadata.format.duration, 'seconds');
        console.log('Bitrate:', metadata.format.bitrate || 'X');
        console.log('Sample Rate:', metadata.format.sampleRate || 'X');
        console.log('Codec:', metadata.format.codec || 'X');
        
        // processing de la cover
        console.log(' ---- ALBUM ART ----- ');
        if (metadata.common.picture && metadata.common.picture.length > 0) {
          const picture = metadata.common.picture[0];
          console.log('✅ Album art found!');
          console.log('Format:', picture.format);
          console.log('Type:', picture.type);
          console.log('Description:', picture.description);
          console.log('Data size:', picture.data.length, 'bytes');
          
          // créé une url pour la cover, sera utilisée plus tard lors du display
          const blob = new Blob([picture.data], { type: picture.format });
          const imageUrl = URL.createObjectURL(blob);
          console.log('url créé', imageUrl);
          
        } else {
          console.log('pas de cover found');
        }
        
        // pour l'exploration
        console.log(' ---- FULL METADATA ----- ');
        console.log(metadata);
        
        this.statusMessage = 'metadata extracted, check console';
        
      } catch (error) {
        console.error(error);
        this.statusMessage = 'error extracting metadata';
      }
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
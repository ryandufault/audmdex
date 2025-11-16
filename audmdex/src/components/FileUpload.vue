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
        console.log('Title:', metadata.common.title || ' ');
        console.log('Artist:', metadata.common.artist || ' ');
        console.log('Album:', metadata.common.album || ' ');
        console.log('Year:', metadata.common.year || ' ');
        console.log('Genre:', metadata.common.genre?.join(', ') || ' ');
        
        // détails techniques
        console.log(' ---- INFOS TECHNIQUES ----- ');
        console.log('Duration:', metadata.format.duration, 'seconds');
        console.log('Bitrate:', metadata.format.bitrate || 'N/A');
        console.log('Sample Rate:', metadata.format.sampleRate || 'N/A');
        console.log('Codec:', metadata.format.codec || 'N/A');
        
        // processing de la cover
        console.log(' ---- ALBUM ART ----- ');
        if (metadata.common.picture && metadata.common.picture.length > 0) { // si binaire présent alors il y a une image
          const picture = metadata.common.picture[0];
          console.log('cover found');
          console.log('Format:', picture.format);
          console.log('Type:', picture.type);
          console.log('Description:', picture.description);
          console.log('Data size:', picture.data.length, 'bytes');
          
          // génère une url avec le binaire de la cover
          const blob = new Blob([picture.data], { type: picture.format });
          const imageUrl = URL.createObjectURL(blob);
          console.log('url créé : ', imageUrl);
          
        } else {
          console.log('pas de cover found'); // si pas de binaire présent pour la cover
        }
        
        // debug
        console.log(' ---- FULL METADATA ----- ');
        console.log(metadata);
        
        this.statusMessage = 'metadata extracted, check console';
        
      } catch (error) {
        console.error('erreur:', error);
        this.statusMessage = 'error extracting metadata';
      }
      
    try {
    const metadata = await musicMetadata.parseBlob(file);
    
    // génère une url avec le binaire de la cover
    let albumArtUrl = null;
    if (metadata.common.picture && metadata.common.picture.length > 0) {
      const picture = metadata.common.picture[0];
      const blob = new Blob([picture.data], { type: picture.format });
      albumArtUrl = URL.createObjectURL(blob);
    }
    
    // data à emit
    const extractedData = {
      // infos fichier
      fileName: file.name,
      fileSize: (file.size / 1024 / 1024).toFixed(2), // (en mb)
      fileType: file.type,
      
      // infos sur audio
      title: metadata.common.title || ' ',
      artist: metadata.common.artist || ' ',
      album: metadata.common.album || ' ',
      genre: metadata.common.genre?.join(', ') || ' ',
      year: metadata.common.year || ' ',
      
      // infos techniques
      duration: metadata.format.duration,
      bitrate: metadata.format.bitrate,
      sampleRate: metadata.format.sampleRate,
      codec: metadata.format.codec,
      codecProfile: metadata.format.codecProfile || 'N/A',
      lossless: metadata.format.lossless,
      numberOfChannels: metadata.format.numberOfChannels,
      tool: metadata.format.tool || 'N/A',
      trackGain: metadata.format.trackGain,
      
      // cover art
      albumArtUrl: albumArtUrl,
      
      // debug
      fullMetadata: metadata
    };
    // emit au parent (l'app)
    this.$emit('metadata-extracted', extractedData);
    this.statusMessage = 'metadata extracted!';
    
  } catch (error) {
    console.error('erreur:', error);
    this.statusMessage = 'error extracting metadata';
  }
}
  }
}
</script>

<style scoped>
.file-container {
  width: 100%;
  min-height: 59.11vh; /* 530px qd 900px de vh */
  background-color: rgba(0, 0, 0, 0);
  border: 0.67vh dotted #cfcfcf; /* 6px qd 900px de vh */
  border-radius: 25px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: border-color 0.3s ease;
}

.file-container:hover {
  border-color: #f1f1f4;
  color: #c3c3c5;
}

.file-container:hover p {
  color: #c3c3c5;
}

.file-container p {
  color: #a1a1a4;
  font-size: 1.2vw;
  transition: color 0.3s ease;
}

/* tablet */
@media (max-width: 1320px) {
  .file-container {
    min-height: 40vh;
    border: 0.59vh dotted #cfcfcf;
    border-radius: 25px;
  }

  .file-container p {
    font-size: 3vw;
  }
}

/* mobile */
@media (max-width: 768px) {
  .file-container {
    border: 1vw dotted #cfcfcf;
    border-radius: 15px;
  }

  .file-container p {
    font-size: 3.25vw;
    padding: 10px;
  }
}

/* sm mobile */
@media (max-width: 420px) {
  .file-container p {
    font-size: 4vw;
  }
}
</style>
<template>
  <div class="metadata-container" :class="{ 'fade-in': isAnimating }">
    <div class="grid-container">
      <!-- display la cover if found -->
      <img 
        v-if="metadata.albumArtUrl" 
        :src="metadata.albumArtUrl" 
        class="album-art"
        alt="Album Art"
        @click="openImageInNewTab"
      />
      <div v-else class="album-art no-art">no album art found // {{ metadata.fileName }}</div>

      <!-- DROPDOWN 1: INFOS SUR AUDIO -->
      <div class="dropdown">
        <input type="checkbox" id="metadata-toggle-1" class="dropdown-toggle" checked @change="dropdownStates[1] = !dropdownStates[1]">
        <label for="metadata-toggle-1" class="dropdown-label">audio infos {{ dropdownStates[1] ? 'v' : '>' }}</label>
        <ul class="dropdown-list">
          <li>title : <strong>{{ metadata.title }}</strong></li>
          <li>artists : <strong>{{ metadata.artist }}</strong></li>
          <li>album : <strong>{{ metadata.album }}</strong></li>
          <li>genre : <strong>{{ metadata.genre }}</strong></li>
          <li>year : <strong>{{ metadata.year }}</strong></li>
          <li>duration : <strong>{{ formatDuration(metadata.duration) }}</strong></li>
          <li>file type : <strong>{{ getFileExtension(metadata.fileName) }}</strong></li>
          <li>bitrate : <strong>{{ formatBitrate(metadata.bitrate) }}</strong></li>
          <li>sample rate : <strong>{{ formatSampleRate(metadata.sampleRate) }}</strong></li>
          <li>bits per sample : <strong>{{ metadata.fullMetadata?.format?.bitsPerSample || 'N/A' }}</strong></li>
          <li>channel layout : <strong>{{ getChannelLayout(metadata.numberOfChannels) }}</strong></li>
        </ul>
      </div>

      <!-- DROPDOWN 2: INFOS TECHNIQUES -->
      <div class="dropdown">
        <input type="checkbox" id="metadata-toggle-2" class="dropdown-toggle" @change="dropdownStates[2] = !dropdownStates[2]">
        <label for="metadata-toggle-2" class="dropdown-label">technical infos {{ dropdownStates[2] ? 'v' : '>' }}</label>
        <ul class="dropdown-list">
          <li>codec : <strong>{{ metadata.codec || 'N/A' }}</strong></li>
          <li>codec profile : <strong>{{ metadata.codecProfile }}</strong></li>
          <li>lossless : <strong>{{ metadata.lossless !== undefined ? metadata.lossless : ' ' }}</strong></li>
          <li>encoder : <strong>{{ metadata.tool || 'N/A' }}</strong></li>
          <li>id3 size : <strong>{{ getID3Size() }}</strong></li>
          <li>copyright flag : <strong>{{ metadata.fullMetadata?.native?.['ID3v2.2']?.copyright || 'false' }}</strong></li>
          <li>track gain : <strong>{{ formatTrackGain(metadata.trackGain) }}</strong></li>
        </ul>
      </div>

      <!-- DROPDOWN 3: INFOS FICHIER -->
      <div class="dropdown">
        <input type="checkbox" id="metadata-toggle-3" class="dropdown-toggle" checked @change="dropdownStates[3] = !dropdownStates[3]">
        <label for="metadata-toggle-3" class="dropdown-label">file infos {{ dropdownStates[3] ? 'v' : '>' }}</label>
        <ul class="dropdown-list">
          <li>file name : <strong>{{ metadata.fileName }}</strong></li>
          <li>file extension : <strong>{{ getFileExtension(metadata.fileName) }} ({{ metadata.fileType }})</strong></li>
          <li>file size (mb) : <strong>{{ metadata.fileSize }}MB</strong></li>
          <li>file duration (s) : <strong>{{ formatDurationSeconds(metadata.duration) }}</strong></li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DisplayMetadata',
  
  props: {
    metadata: {
      type: Object,
      required: true
    }
  },
  
  data() {
  return {
    isAnimating: false,
    dropdownStates: {
      1: true,  // audio infos open
      2: false, // technical infos closed
      3: true   // file infos open
    }
  }
  },
  
  watch: { // watch si la metadata change
    metadata: {
      handler(newVal, oldVal) {
        // white bg (true)
        this.isAnimating = true;
        
        // après delai
        setTimeout(() => {
          this.isAnimating = false;
        }, 300); // delai avant de fade out
      },
      deep: true,
      immediate: true // run quand la page load aussi
    }
  },
  
  mounted() {
    // trigger l'animation quand mounted
    this.isAnimating = true;
  },
  
  methods: {
    formatDuration(seconds) {
      if (!seconds) return ' ';
      const mins = Math.floor(seconds / 60);
      const secs = Math.floor(seconds % 60);
      return `${mins}m${secs}s`;
    },
    
    formatDurationSeconds(seconds) {
      if (!seconds) return ' ';
      return `${Math.floor(seconds)}s`;
    },
    
    formatBitrate(bitrate) {
      if (!bitrate) return ' ';
      if (bitrate >= 1000) {
        return `${Math.floor(bitrate / 1000)}kbps`;
      }
      return `${bitrate}bps`;
    },
    
    formatSampleRate(sampleRate) {
      if (!sampleRate) return ' ';
      if (sampleRate >= 1000) {
        return `${sampleRate / 1000}kHz`;
      }
      return `${sampleRate}Hz`;
    },
    
    formatTrackGain(trackGain) {
      if (trackGain === undefined || trackGain === null) return 'N/A';
      return `${trackGain}dB`;
    },
    
    getFileExtension(fileName) {
      if (!fileName) return ' ';
      return fileName.split('.').pop().toLowerCase();
    },
    
    getChannelLayout(channels) {
      if (!channels) return ' ';
      if (channels === 1) return 'mono';
      if (channels === 2) return 'stereo';
      return `${channels} channels`;
    },
    
    getID3Size() {
      // trouve les 3 tags (si dispo)
      const id3Tags = this.metadata.fullMetadata?.native?.['ID3v2.2'] || 
                     this.metadata.fullMetadata?.native?.['ID3v2.3'] || 
                     this.metadata.fullMetadata?.native?.['ID3v2.4'];
      
      if (id3Tags && Array.isArray(id3Tags)) {
        // estimation sur le nombre de tags found
        return `~${id3Tags.length} tags`;
      }
      return 'N/A';
    },

    openImageInNewTab() {
      window.open(this.metadata.albumArtUrl, '_blank');
    }
  }
}
</script>

<style scoped>
.metadata-container {
  width: 100%;
  min-height: auto;
  background-color: rgba(255, 255, 255, 0);
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-top: 2vw;
  transition: background-color 0.5s ease-in-out;
}

.metadata-container.fade-in {
  background-color: rgba(255, 255, 255, 0.3); /* anim */
}

.grid-container {
  display: grid;
  grid-template-columns: auto auto auto;
  grid-template-rows: auto auto;
  gap: 0.5vw;
  width: 100%;
}

.album-art {
  grid-row: 1 / 1;
  aspect-ratio: 1;
  width: 20.5vw;
  background-color: #333;
  border: 0.12vw solid #cfcfcf;
  cursor: pointer;
  object-fit: cover;
  text-align: center;
  font-size: 1vw;
}

.album-art.no-art {
  display: flex;
  align-items: center;
  justify-content: center;
  color: #a1a1a4;
}

.dropdown {
  width: auto;
}

.dropdown:nth-child(2) {
  grid-column: 2;
  grid-row: 1;
  align-self: start;
}

.dropdown:nth-child(3) {
  grid-column: 3;
  grid-row: 1;
  align-self: start;
}

.dropdown:nth-child(4) {
  grid-column: 2 / 4;
  grid-row: 2;
  align-self: start;
}

.dropdown-toggle {
  display: none;
}

.dropdown-label {
  font-size: 1vw;
  display: block;
  padding: 0.625vw;
  color: #cfcfcf;
  cursor: pointer;
  user-select: none;
  border-bottom: 0.12vw solid #cfcfcf;
}

.dropdown-label:hover {
  color: #e6e6e7;
  border-bottom: 0.12vw solid #e6e6e7;
  transition: border-bottom, color 0.3s ease;
}

.dropdown-list {
  max-height: 0;
  overflow: hidden;
  list-style: none;
  padding: 0;
  margin: 0;
}

.dropdown-toggle:checked ~ .dropdown-list {
  max-height: 31.25vw;
  padding: 0.625vw;
}

.dropdown-list li {
  font-size: 1vw;
  color: #a1a1a4;
  padding: 0.313vw 0;
}

.dropdown-list li:hover {
  color: #cfcfcf;
  transition: color 0.3s ease;
}

.dropdown-list li strong {
  color: #cfcfcf;
  font-weight: normal;
}
</style>
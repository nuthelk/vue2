<template>
  <div>
    <h1>Upload and List Files</h1>
    <input type="file" @change="handleFileUpload" />
    <button @click="uploadFile">Upload</button>
    <ul>
      <li v-for="file in files" :key="file.fileName">
        <a @click="downloadFile(file.fileName)">{{ file.fileName }}</a>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';

interface FileItem {
  fileName: string;
}

export default defineComponent({
  data() {
    return {
      selectedFile: null as File | null,
      files: [] as FileItem[]
    };
  },
  methods: {
    handleFileUpload(event: Event) {
      const target = event.target as HTMLInputElement;
      if (target.files && target.files[0]) {
        this.selectedFile = target.files[0];
      }
    },
    async uploadFile() {
      if (!this.selectedFile) return;

      const formData = new FormData();
      await formData.append('file', this.selectedFile);
    
      try {
        const response = await axios.post('https://backend-5n34.onrender.com/upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        });
        console.log(response.data);
        this.fetchFiles();
      } catch (error) {
        console.error(error);
      }
    },
    async fetchFiles() {
      try {
        const response = await axios.get('https://backend-5n34.onrender.com/files');
        this.files = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    async downloadFile(fileName: string) {
      const key = prompt('Enter the download key:');
      if (!key) return;

      try {
        const response = await axios.get('https://backend-5n34.onrender.com/download', {
          params: { file: fileName, key },
          responseType: 'blob'
        });
        const url = window.URL.createObjectURL(new Blob([response.data]));
        const link = document.createElement('a');
        link.href = url;
        link.setAttribute('download', fileName);
        document.body.appendChild(link);
        link.click();
      } catch (error) {
        console.error(error);
      }
    }
  },
  mounted() {
    this.fetchFiles();
  }
});
</script>

<template>
  <div>
    <input type="file" multiple @change="handleFileUpload" accept="image/*" />
    <button @click="uploadImage">Загрузить</button>
    
  </div>
</template>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      selectedFile: null,
    };
  },
  
  methods: {
    handleFileUpload(event) {
      this.selectedFile = event.target.files;
    },
    uploadImage() {
      const formData = new FormData();
      formData.append('image', this.selectedFile);
      console.log(this.selectedFile)

      axios
        .post('http://localhost:3000/images', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        })
        .then(response => {
          console.log(response);
          this.imageUrl = response.data.imageUrl;
        })
        .catch(error => {
          console.error( error);
        });
    },
    
  },
};
</script>
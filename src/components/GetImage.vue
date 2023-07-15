<template>
  <div>
    <img :src="imageUrl" alt="Загруженное изображение" v-if="imageUrl" />
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      imageUrl: null,
    };
  },
  mounted() {
    this.fetchImage();
  },
  methods: {
    fetchImage() {
      axios
        .get('https://dog.ceo/api/breeds/image/random')
        .then(response => {
          console.log(response)
          this.imageUrl = response.data.message;
        })
        .catch(error => {
          console.error(error);
        });
    },
  },
};
</script>
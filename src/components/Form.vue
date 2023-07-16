<template>
  <q-layout class="column" view="hHh lpR fFf">

    <q-page-container>
      <q-form
        class="q-gutter-md q-ma-xl row wrap justify-center"
        @reset="onReset"
        @submit="onSubmit"
      >
        <q-input
          v-model="title"
          :rules="[ val => val && val.length > 0 || 'Please type something']"
          class="col-md-6 text-h6"
          filled
          label="Title"
          lazy-rules
        />

        <q-select v-model="model" :options="options" class="col-md-6 text-h6" filled label="Category"/>
<!--        <q-uploader-->
<!--          bordered-->
<!--          class="col-md-6 text-h6"-->
<!--          color="purple"-->
<!--          flat-->
<!--          label="Upload files"-->
<!--          v-model="body"-->
<!--          square-->
<!--          url="http://localhost:3000/posts"-->
<!--        />-->
        <div class="col-md-6">
          <input type="file" multiple @change="handleFileUpload" accept="image/*" />
        </div>
        <div class="col-md-6 ">
          <q-btn color="primary" label="Submit" size="17px" type="submit"/>
          <q-btn class="q-ml-sm" color="primary" flat label="Reset" size="17px" type="reset"/>
        </div>
      </q-form>
      <get-image/>
    </q-page-container>

  </q-layout>
</template>
<script setup>
import {useQuasar} from "quasar";
import { ref} from "vue";
import axios from 'axios';

import GetImage from "./GetImage.vue";


const $q = useQuasar()

const title = ref(null);
const body = ref(null);
const model = ref(null);
const selectedFile = ref(null)
const options = [
  'Браслеты', 'Серёжки', 'Цепочки', 'Для мужика', 'Для прекрасной половины человечества'
]


function onSubmit() {


  axios.post('http://localhost:3000/posts', {
    title: title.value,
    body: model.value,
  })
    .then((response) => console.log(response.data))

  const formData = new FormData();
  formData.append('image', selectedFile.value);
  console.log(selectedFile.value)

  axios
    .post('http://localhost:3000/images', formData, {
      headers: {
        'Content-Type': 'multipart/form-data',
      },
    })
    .then(response => {
      console.log(response);
      let imageUrl = response.data.imageUrl;
    })
    .catch(error => {
      console.error( error);
    });


}
function handleFileUpload(event) {
  selectedFile.value = event.target.files;
}
// function uploadImage() {
//   const formData = new FormData();
//   formData.append('image', this.selectedFile);
//   console.log(this.selectedFile)
//
//   axios
//     .post('http://localhost:3000/posts/img', formData, {
//       headers: {
//         'Content-Type': 'multipart/form-data',
//       },
//     })
//     .then(response => {
//       console.log(response);
//       this.imageUrl = response.data.imageUrl;
//     })
//     .catch(error => {
//       console.error( error);
//     });
// }

function onReset() {
  title.value = null
  body.value = null
}
</script>
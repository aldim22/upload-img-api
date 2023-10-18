<template>
  <div>
      <div v-if="showAlert" v-bind:class="['alert', 'alert-' + alertType]">
        <button type="button" class="close" @click="closeAlert">&times;</button>
        {{ alertMessage }}
      </div>
    </div>

  <div>
    <input class="form-control" type="file" @change="uploadImage" />
    <div v-if="uploading">
      <progress style="width: 100%;" :value="uploadProgress" max="100"></progress>
    </div>
    <!-- <img :src="imageUrl" alt="Uploaded Image" v-if="imageUrl" /> -->
  </div>


  <h2 style="text-align: left;margin-top: 20px;">My gallery</h2>
  <div class="card">
    <div class="card-header">list image</div>
    <div class="card-body">
      <div class="row" v-if="savedImageUrls.length > 0">
        <div v-for="(imageUrl, index) in savedImageUrls" :key="index"  class="col-lg-4">
          <div class="card mt-3">
            <div class="card-body">
              <img style="width: 100%;height: 399px;" :src="imageUrl" alt="Saved Image" />
            </div>
          </div>
        </div>
      </div>
      <div v-else>
        <p><i>gambar tidak ada, silakan lakukan upload</i> </p>
      </div>
    </div>
  </div>
  
  

</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      imageUrl: null,
      uploading: false,
      uploadProgress: 0,
      savedImageUrls: JSON.parse(localStorage.getItem('savedImageUrls')) || [],

      showAlert: false,
      alertType: 'primary', // Change to 'danger', 'warning', etc. as needed
      alertMessage: '',
    };
  },
  methods: {
    closeAlert() {
      this.showAlert = false;
    },
    uploadImage(event) {
      const file = event.target.files[0];
      const formData = new FormData();
      formData.append('image', file);

      this.uploading = true;
      const allowedFileTypes = ['image/jpeg', 'image/jpg', 'image/bmp', 'image/png', 'image/gif'];
      if (allowedFileTypes.includes(formData.get('image').type)) {
        axios
          .post('https://api.imgbb.com/1/upload?key=211b1c07f7a274847ebafdaa7f49aa56', formData, {
            onUploadProgress: (progressEvent) => {
              this.uploadProgress = Math.round((progressEvent.loaded / progressEvent.total) * 100);
            },
          })
          .then((response) => {
            if (formData.get('image').size > 2 * 1024 * 1024) {
              this.alertType = 'danger';
              this.alertMessage = 'File size is too large. Please upload an image smaller than 2MB.';
              this.showAlert = true;
              return;
            }
            this.alertType = 'success';
            this.alertMessage = 'Gambar berhasil diupload';
            this.showAlert = true;

            this.imageUrl = response.data.data.url;

            this.savedImageUrls.push(this.imageUrl);
          // Save the updated array to localStorage
          localStorage.setItem('savedImageUrls', JSON.stringify(this.savedImageUrls));

          })
          .catch((error) => {
            this.alertType = 'danger';
            this.alertMessage = 'Upload error';
            this.showAlert = true;
            console.error('Image Upload error', error);
          })
          .finally(() => {
            this.uploading = false;
          });
      }else{
        this.alertType = 'danger';
        this.alertMessage = 'Invalid file type. Please upload a valid image file.';
        this.showAlert = true;
      }
    },
  },
};
</script>
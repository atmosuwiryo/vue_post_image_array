<template>
  <div>
    <h1>Post Data</h1>
    <form @submit.prevent="postData">
      <label for="name">Name:</label>
      <input type="text" id="name" v-model="name" required><br>

      <label for="description">Description:</label>
      <input type="text" id="description" v-model="description" required><br>

      <label for="sku">SKU:</label>
      <input type="text" id="sku" v-model="sku" required><br>

      <label for="mainImage">Main Image:</label>
      <input type="file" id="mainImage" @change="handleMainImage"><br>

      <label for="images">Images:</label>
      <input type="file" id="images" multiple @change="handleImages"><br>

      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      name: '',
      description: '',
      sku: '',
      mainImage: null,
      images: []
    };
  },
  methods: {
    handleMainImage(event) {
      this.mainImage = event.target.files[0];
    },
    handleImages(event) {
      this.images = Array.from(event.target.files);
    },
    async postData() {
      console.log('posting data');
      const formData = new FormData();
      formData.append('name', this.name);
      formData.append('description', this.description);
      formData.append('sku', this.sku);
      formData.append('mainImage', this.mainImage);
      // formData.append('images', this.images)
      this.images.forEach((image) => {
        formData.append('images', image);
      });

      try {
        const response = await axios.post('http://localhost:3011/products', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        });
        console.log(response.data);
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>

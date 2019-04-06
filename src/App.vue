<template>
  <div class="container-fluid p-4">
    <div class="row">
      <div class="col-md-3 text-center">
        <img alt="Vue logo" src="./assets/logo.png">
        <input type="file" class="form-control" v-on:change="upload">
      </div>
      <div class="col-md-9">
        <div class="row">
          <div v-for="thumb in thumbs" :key="thumb.publicId" class="col-md-4">
            <thumb v-bind:thumb="thumb"></thumb>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import thumb from "./components/thumb-item.component.vue";
import { thumbs } from "./example";

export default {
  name: "app",
  components: {
    thumb
  },
  data() {
    return {
      thumb,
      thumbs,
      cloudinary: {
        uploadPreset: ``,
        apiKey: '',
        cloudName: '',
      },
      loading: false
    };
  },
  computed: {
    cloudinaryURL() {
      return `https://api.cloudinary.com/v1_1/${this.cloudinary.cloudName}/image/upload`
    }
  },
  methods: {
    async upload(e) {
      const file = e.target.files;
      const formData = new FormData();
      formData.append('file', file[0]);
      formData.append('upload_preset', this.cloudinary.uploadPreset);
      formData.append('tags', 'vue, tutorial');
      // For debug uncomment the following
      // for(var pair of formData.entries()) {
      //   console.log(pair[0]+', '+pair[1]);
      // }
      try {
        const res = await axios.post(this.cloudinaryURL, formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        });
        console.log(res);
        this.thumbs.unshift({
          publidId: res.data.public_id,
          url: res.data.url
        });
      } catch(e) {
        console.log(e)
      }
    }
  }
};
</script>
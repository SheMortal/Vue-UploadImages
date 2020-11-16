<template>
  <div class="file">
    <form @submit.prevent="onSubmit" enctype="multipart/form-data">
      <div class="fields">
        <label>Upload File</label>
        <br />
        <input type="file" ref="file" v-on:change="onSelect" />
      </div>
      <br />
      <div class="fields">
        <button>Submit</button>
      </div>
      <div class="message">
        {{ message }}
      </div>
    </form>
  </div>
</template>

<script>
// import axios
import axios from "axios";

export default {
  name: "FileUpload",
  data() {
    return {
      file: "",
      message: "",
    };
  },
  methods: {
    onSelect() {
      const allowedTypes = ["image/jpgeg", "image/jpg", "image/png"];
      // get file using $refs.file
      const file = this.$refs.file.files[0];
      this.file = file;
      if (!allowedTypes.includes(file.type)) {
        this.message = "Only images are required!";
      }
      if (file.size > 900000) {
        this.message = "Too large, max size allowed is 500KB";
      }
    },
    async onSubmit() {
      // to send form data
      const formData = new FormData();
      formData.append("file", this.file);
      try {
        // backend request
        await axios.post(" http://localhost:5000/uploads", formData);
        this.message = "File successfully uploaded!";
      } catch (err) {
        console.log(err);
        this.message = err.message.data.error;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>

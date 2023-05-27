<template>
  <div class="container">
      <div v-if="prediction">its a {{ this.prediction }}</div>
      <div class="upload-button">
      <input
              ref="fileInput"
              class="input-custom"
              id="file-input"
              type="file"
              @change="handleChange"
      />
      <label for="file-input" class="label-custom">Upload</label>
  </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Emotionizer",
  data() {
    return {
      file: null,
      isUpload: true,
      prediction: "",
    };
  },
    components: {
    },
  methods: {
      handleChange(event) {
          this.file = event.target.files[0];
          this.getEmotion(event);
      },

      getEmotion(event) {
          const selectedFile = event.target.files[0];
          console.log(selectedFile)
          const formData = new FormData();
          formData.append("image", selectedFile);

          axios
              .post("http://localhost:5000/predict", formData, {
                  headers: {
                      "Content-Type": "multipart/form-data",
                  },
              })
              .then((response) => {
                  this.prediction = response.data.prediction;
              })
              .catch((error) => {
                  console.error(error);
              });
      },
  },
  computed: {
    fileUrl() {
      if (this.file) {
        return URL.createObjectURL(this.file);
      }
      return null;
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
}

.label-custom {
    display: flex;
    width: 5rem;
    height: 1.5rem;
    padding: 5px;
    background-color: #00BFFF;
    border-radius: 5px;
    transition: all 0.4s;
    align-items: center;
    justify-content: center;
}

.label-custom:hover,.label-custom:active , .label-custom:focus {
    color: #FFA500;
    background-color: #333333;
    border-radius: 5px;
    padding: 5px;
    cursor: pointer;
}

.input-custom {
    display: none;
}
</style>

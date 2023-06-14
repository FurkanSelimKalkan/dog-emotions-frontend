<template>
  <div class="container">
    <h2 class="emotion-text">Please upload a picture of your Dog</h2>
    <h3 class="emotion-text" v-if="prediction">Emotion: {{ this.prediction }}</h3>
    <h3 class="emotion-text"  v-if="confidence">Confidence: {{ this.confidence * 100 }}%</h3>
    <div class="dog-image-container" v-if="fileUrl">
      <img class="dog-image" :src="fileUrl" v-if="fileUrl" />
    </div>
    <div class="emotion-text" v-if="prediction"> {{ getRandomResponse(this.prediction)}} </div>
    <div class="upload-buttons">
      <button class="label-custom" @click="handleUpload">Upload</button>
      <input ref="fileInput" class="input-custom" id="file-input" type="file" style="display: none" @change="handleChange" />

      <button class="label-custom" @click="callAPI">Submit</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { useEmotionStore } from '../stores/emotionStore.js';

export default {
  name: "Emotionizer",
  data() {
    return {
      file: null,
      isUpload: true,
      prediction: "",
      confidence: ""
    };
  },
  methods: {
    handleUpload() {
      this.$refs.fileInput.click();
    },
    handleChange(event) {
      this.file = event.target.files[0];
      this.getEmotion(event);
    },
    getEmotion(event) {
      const selectedFile = event.target.files[0];
      console.log(selectedFile);
      const formData = new FormData();
      formData.append("image", selectedFile);

      axios
          .post("http://localhost:5000/predict", formData, {
            headers: {
              "Content-Type": "multipart/form-data"
            }
          })
          .then((response) => {
            this.prediction = response.data.prediction;
            this.confidence = response.data.confidence.toFixed(2);
            this.$forceUpdate();
          })

          .catch((error) => {
            console.error(error);
          });
    },
    getRandomResponse(emotion) {
      let emotionStore = useEmotionStore();
      let responses;

      switch (emotion) {
        case "angry":
          responses = emotionStore.angryResponses;
          break;
        case "happy":
          responses = emotionStore.happyResponses;
          break;
        case "sad":
          responses = emotionStore.sadResponses;
          break;
        case "relaxed":
          responses = emotionStore.relaxedResponses;
          break;
        default:
          return "No emotion detected. Please try again.";
      }

      const randomIndex = Math.floor(Math.random() * responses.length);
      return responses[randomIndex];
    },
    callAPI() {
      if (this.file) {
        const formData = new FormData();
        formData.append("image", this.file);
        // CHANGE THIS WITH PC IP ADDRESS INSTEAD OF LOCALHOST
        axios
            .post(`http://${import.meta.env.VITE_APP_IP}:5000/predict`, formData, {
              // .post("http://localhost:5000/predict", formData, {
              headers: {
                "Content-Type": "multipart/form-data"
              }
            })
            .then((response) => {
              this.prediction = response.data.prediction;
              this.confidence = response.data.confidence.toFixed(2);
              this.$forceUpdate();
            })
            .catch((error) => {
              console.error(error);
            });
      }
    }
  },
  computed: {
    fileUrl() {
      if (this.file) {
        return URL.createObjectURL(this.file);
      }
      return null;
    }
  }
};
</script>

<style scoped>
.container {
  width: 390px;
  margin: 0 auto;
}


.dog-image-container {
  display: flex;
  border: 2px solid #333333;
  border-radius: 10px;
  max-width: 75%;
  height: auto;
  margin: 7px auto;
  padding: 7px;
}


.dog-image {
  border-radius: 10px;
  max-width: 100%;
  height: auto;
}

.label-custom{
  display: flex;
  width: 4rem;
  height: 1.2rem;
  background-color: #00bfff;
  border-radius: 5px;
  border-color: transparent;
  transition: all 0.4s;
  align-items: center;
  justify-content: center;
  color: black;
  font-weight: normal;
  font-size: 16px;
  padding: 20px 70px 20px 70px;
}

.label-custom:hover,
.label-custom:active,
.label-custom:focus{
  color: #ffa500;
  background-color: #333333;
  border-radius: 5px;
  cursor: pointer;
}

.upload-buttons {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-top: 10px;
  justify-content: center;
  position: absolute;
  bottom: 20px; /* Adjust the value as needed */
  left: 50%;
  transform: translateX(-50%);
}

.emotion-text {
  text-align: center;
}



.input-custom {
  display: none;
}

</style>

<template>
  <div class="container">
    <h2>
      Please upload a picture with only one dog (not several)
    </h2>
    <h3 v-if="prediction">Identified emotion: {{ this.prediction }}</h3>
    <div class="dog-image-container" v-if="fileUrl">
    <img class="dog-image" :src="fileUrl" v-if="fileUrl" />
    </div>
      <div v-if="prediction"> {{ getRandomResponse(this.prediction)}} </div>
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
      angryResponses : [
        "Your dog seems to be angry. It may be best to give them some space.",
        "Looks like your dog is feeling angry. Try to identify any triggers or sources of frustration.",
        "The emotion detected in your dog is anger. Take precautions and ensure everyone's safety.",
        "An angry dog needs calm and patience. Provide a peaceful environment for them.",
        "Anger can be a sign of discomfort or distress in dogs. Check for any underlying issues.",
        "When dogs are angry, it's important to avoid confrontations and seek professional advice if needed.",
        "An angry dog might benefit from some quiet time and positive reinforcement training.",
        "Recognizing anger in dogs is crucial for their well-being. Take steps to address their needs.",
        "If your dog is displaying signs of anger, consider consulting with a certified dog behaviorist.",
        "An angry dog might require additional socialization and training to address their emotions."
      ],
      happyResponses : [
        "Your dog seems to be happy. Keep up the good work!",
        "Looks like your dog is feeling happy. Keep doing what you're doing!",
        "The emotion detected in your dog is happiness. Keep up the good work!",
        "A happy dog is a healthy dog. Keep up the good work!",
        "Happiness is a sign of a healthy dog. Keep up the good work!",
        "When dogs are happy, it's important to keep doing what you're doing!",
        "A happy dog is a sign of a healthy dog. Keep up the good work!",
        "Recognizing happiness in dogs is crucial for their well-being. Keep up the good work!",
        "If your dog is displaying signs of happiness, consider consulting with a certified dog behaviorist.",
        "A happy dog might require additional socialization and training to address their emotions."
      ],
      sadResponses : [
        "Your dog seems to be sad. It may be best to give them some space.",
        "Looks like your dog is feeling sad. Try to identify any triggers or sources of frustration.",
        "The emotion detected in your dog is sadness. Take precautions and ensure everyone's safety.",
        "A sad dog needs calm and patience. Provide a peaceful environment for them.",
        "Sadness can be a sign of discomfort or distress in dogs. Check for any underlying issues.",
        "When dogs are sad, it's important to avoid confrontations and seek professional advice if needed.",
        "A sad dog might benefit from some quiet time and positive reinforcement training.",
        "Recognizing sadness in dogs is crucial for their well-being. Take steps to address their needs.",
        "If your dog is displaying signs of sadness, consider consulting with a certified dog behaviorist.",
        "A sad dog might require additional socialization and training to address their emotions."
      ],
      relaxedResponses : [
        "Your dog seems to be relaxed and content. It's a good sign!",
        "Looks like your dog is feeling relaxed. Give them some well-deserved rest and relaxation.",
        "A relaxed dog is a happy dog. Enjoy the peaceful moments together!",
        "When dogs are relaxed, it's a great time for bonding and gentle interactions.",
        "A relaxed dog indicates a calm and harmonious environment. Keep up the good work!",
        "Seeing your dog relaxed is a joy. Cherish the tranquility in their presence.",
        "Make the most of your dog's relaxed state by engaging in soothing activities together.",
        "A relaxed dog is an indication of trust and comfort. Keep fostering that positive environment.",
        "Take a moment to appreciate the serenity your dog brings when they're relaxed.",
        "Relaxed dogs are often more receptive to training and socialization. Utilize this time for positive experiences."
      ]

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
     getRandomResponse(emotion) {
  let responses;

  switch (emotion) {
    case "angry":
      responses = this.angryResponses;
      break;
    case "happy":
      responses = this.happyResponses;
      break;
    case "sad":
      responses = this.sadResponses;
      break;
    case "relaxed":
      responses = this.relaxedResponses;
      break;
    default:
      return "No emotion detected. Please try again.";
  }

  const randomIndex = Math.floor(Math.random() * responses.length);
  return responses[randomIndex];
}

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

.dog-image-container {
  display: inline-block;
  border: 2px solid #333333;
  border-radius: 10px;
  max-width: 100%;
  height: auto;
  margin-top: 7px;
  padding: 7px;
}

.dog-image {
  border-radius: 10px;
  max-width: 100%;
  height: auto;
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

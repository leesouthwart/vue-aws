<template>
  <div class="answer">
    <div class="answer-inner">
      <div class="image-top">
        <input class="hidden" accept="image/*" type="file" ref="fileInput">
        <img class="main-img" ref="image" :src="imageUrl" alt="placeholder" @click="uploadImage">
        <div v-if="imageChanged" class="remove-container">
          <img class="remove" @click="reset(false)" :src="require('@/assets/remove.svg')">
        </div>
      </div>

      <div class="answer-text">
        <input v-model="inputValue" placeholder="Write here" type="text" @blur="showInstruction" @keyup.enter="submit">
      </div>
    </div>
  </div>
</template>

<script>

import { Notyf } from 'notyf';
import 'notyf/notyf.min.css';

const notyf = new Notyf({
  duration: 4000
});

export default {
  name: 'CreateAnswerComponent',
  data() {
    return {
      inputValue: '',
      imageUrl: require('@/assets/placeholder.png'),
      imageChanged: null,
      seenInstructions: false,
    }
  },

  methods: {
    uploadImage() {
      // Trigger file input click event
      this.$refs.fileInput.click();
    },
    handleFileUpload(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();

        reader.onload = () => {
          // Update imageUrl to display the uploaded image
          this.imageUrl = reader.result;
        };

        reader.readAsDataURL(file);

        // add width constraint to image
        this.$refs.image.classList.add('image-width');
        this.imageChanged = true;
      }
    },
    submit() {
      if (this.imageChanged && this.inputValue !== '') {
        this.emitValues();

      } else if (!this.imageChanged || this.inputValue === '') {
        // Image not changed or input value is empty
        if (!this.imageChanged && this.inputValue === '') {
          // Do nothing here
        } else {
          // Image not changed or input value is empty, but one of them is true
          notyf.error('Please add both an image and an answer')
        }
      }
    },
    emitValues() {
      this.$emit('answer-created', { imageURL: this.imageUrl, text: this.inputValue });
      this.reset();
    },
    reset(removeInput = true) {
      this.imageChanged = false;
      this.imageUrl = require('@/assets/placeholder.png');
      this.$refs.image.classList.remove('image-width');

      if(removeInput) {
        this.inputValue = '';
      }
    },
    showInstruction() {
      if(!this.seenInstructions) {
        notyf.error('Press Enter to Create Answer');
        this.seenInstructions = true;
      }
    }
  },

  mounted() {
    this.$refs.fileInput.addEventListener('change', this.handleFileUpload);
  },
  beforeUnmount() {
    // Remove event listener
    this.$refs.fileInput.removeEventListener('change', this.handleFileUpload);
  }
}
</script>

<style scoped lang="scss">
.answer {
  width: 25%;
  margin: 0;
  padding: 10px;
  box-sizing: border-box;

  @media screen and (max-width: 999px) {
    width: 33.333%;
  }

  @media screen and (max-width: 768px) {
    width: 50%;
  }

  .answer-inner {
    border: 1px solid #E3E1E1;
    box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.09);
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  .image-top {
    border-bottom: 1px solid #E3E1E1;
    flex-grow: 1;
    display: flex;
    position: relative;

    .main-img {
      height: auto;
      margin: auto;
    }

    .image-width {
      width: 100%;
    }

    .remove-container {
      position: absolute;
      top: 10px;
      left: 10px;
      display: flex;
      justify-content: center;
      background-color: #888888;
      border-radius: 50%;
      padding: 7px;
      width: 15px;
      height: 15px;
      z-index: 99;

      .remove {
        &:hover {
          cursor: pointer;
        }
      }
    }
  }

  .answer-text {
    padding: 0 10px;
    display: flex;

    input {
      margin-top: 1rem;
      margin-bottom: 1rem;
      width: 100%;
    }
  }
}

.hidden {
  display: none;
}

</style>
<template>
  <div class="answer">
    <div class="answer-inner">
      <div class="image-top">
        <img class="main-img" :src="answer.imageURL" alt="Answer Image">
        <div class="checkbox-container">
          <input :name="'custom-checkbox' + index" :id="'custom-checkbox' + index" type="checkbox" v-model="checked" class="custom-checkbox" @change="emitIndex" />
          <label :for="'custom-checkbox' + index"></label>
        </div>

        <div class="remove-container">
          <img class="remove" @click="remove" :src="require('@/assets/remove.svg')">
        </div>
      </div>

      <div class="answer-text">
        <input v-model="inputValue" type="text" @blur="submit" @keyup.enter="submit">
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: 'AnswerComponent',
  props: {
    answer: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
    }
  },

  data() {
    return {
      inputValue: '',
      checked: false,
    }
  },

  mounted() {
    this.inputValue = this.answer.text;
  },

  methods: {
    submit() {
      this.$emit('answer-updated', { text: this.inputValue, index: this.index });
    },

    remove() {
      this.$emit('answer-deleted', this.index);
    },

    emitIndex() {
      if (this.checked) {
        this.$emit('index-checked', this.index);
      } else {
        this.$emit('index-unchecked', this.index);
      }
    }
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
      width: 100%;
      height: auto;
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

      .remove {
        &:hover {
          cursor: pointer;
        }
      }
    }
  }

  .answer-text {
    padding: 0 10px;
  }

  input {
    margin: 1rem 0;
    border: 1px solid transparent;

    &:hover, :focus {
      border: 1px solid #E3E1E1;
    }
  }

  .checkbox-container {
    position: absolute;
    top: 10px;
    right: 10px;
  }

  .custom-checkbox {
    display: none;
  }

  .checkbox-container label {
    display: inline-block;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 3px;
    cursor: pointer;

    &:after {
      content: '';
      display: block;
      width: 14px;
      height: 14px;
      margin: 3px;
      border-radius: 2px;
    }
  }

  .checkbox-container input:checked + label {
    background-color: #475CAF;

    &:after {
      content: '\2713';
      color: #fff;
      font-size: 14px;
      text-align: center;
      line-height: 14px;
    }
  }
}

</style>
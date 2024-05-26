<template>
  <div>
    <container-component>
      <div class="answers-list-component">
        <input type="text" v-model="question">

        <div class="answers-grid" v-if="answerData && answerData.answers">
          <answer-component
              @answer-updated="handleAnswerUpdated"
              @answer-deleted="handleAnswerDeleted"
              @index-checked="handleIndexChecked"
              @index-unchecked="handleIndexUnchecked"
              :answer="answer" v-for="(answer, index) in answerData.answers"
              :index="index" :key="index" />

          <create-answer-component @answer-created="handleAnswerCreated" />
        </div>

      </div>
    </container-component>
  </div>
</template>

<script>

import ContainerComponent from "@/components/Container/ContainerComponent.vue";
import AnswerComponent from '@/components/AnswersList/Answer/AnswerComponent.vue'
import CreateAnswerComponent from "@/components/AnswersList/CreateAnswer/CreateAnswerComponent.vue";
import { Notyf } from 'notyf';
import 'notyf/notyf.min.css';

const notyf = new Notyf();


export default {
  name: 'AnswersListComponent',
  components: {
    CreateAnswerComponent,
    AnswerComponent,
    ContainerComponent,
  },

  data() {
    return {
      answerData: null,
      question: '',
      checkedAnswers: [],
    }
  },

  mounted() {
    this.fetchInitialData();
  },

  watch: {
    answerData: {
      handler(newAnswerData) {
        localStorage.setItem('answerData', JSON.stringify(newAnswerData));
      },
      deep: true
    },
    question: {
      handler(newQuestion) {
        this.answerData.question.text = newQuestion;
      }
    }
  },

  methods: {
    async fetchInitialData() {


      if(!localStorage.getItem('answerData')) {
        try {
          // Fetch from s3
          const res = await fetch('https://leestorage.s3.eu-north-1.amazonaws.com/data.json');

          if(!res.ok) {
            throw new Error('API Call failed.');
          }

          // Store into Data - Triggers watcher to store into localStorage.
          this.answerData = await res.json();
          this.question = this.answerData.question.text;


        } catch (error) {
          console.error('Error fetching data from S3:', error.message);
        }
      } else {
        try {
          this.answerData = JSON.parse(localStorage.getItem('answerData'));
          this.question = this.answerData.question.text;
        } catch {
          console.log('Error getting data from local storage.');
        }
      }
    },
    handleAnswerCreated(answer) {
      this.answerData.answers[Object.keys(this.answerData.answers).length] = answer;
      notyf.success('Answer Created!');
    },
    handleAnswerUpdated(answer) {
      this.answerData.answers[answer.index].text = answer.text;
      notyf.success('Answer Updated!');
    },
    handleAnswerDeleted(index) {
      this.answerData.answers.splice(index, 1);
      notyf.success('Answer Deleted!');
    },
    handleIndexChecked(index) {
      this.checkedAnswers.push(index);
    },
    handleIndexUnchecked(index) {
      this.checkedAnswers = this.checkedAnswers.filter(item => item !== index);
    }
  }
}
</script>

<style scoped lang="scss">
.answers-list-component {
  margin: 60px 0;
  padding: 2rem;
  background-color: white;
  border-radius: 5px;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.09);
}

.answers-grid {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  margin-top: 30px;
}

input {
  font-size: 21px;
  width: 100%;
  border: 1px solid transparent;

  &:hover {
    border: 1px solid #E3E1E1;
  }
}
</style>
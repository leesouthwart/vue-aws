<template>
  <header-component />
  <answers-list-component />
  <footer-component />
</template>

<script>
import HeaderComponent from './components/Header/HeaderComponent.vue'
import AnswersListComponent from "@/components/AnswersList/AnswersListComponent.vue";
import FooterComponent from "@/components/Footer/FooterComponent.vue";

export default {
  name: 'App',
  components: {
    AnswersListComponent,
    HeaderComponent,
    FooterComponent
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

body {
  padding: 0;
  margin: 0;
  background-color: #F8F8F9;
}
</style>

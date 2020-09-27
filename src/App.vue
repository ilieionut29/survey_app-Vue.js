<template>
  <div id="app">
    <app-survey-content></app-survey-content>
    <app-survey-result></app-survey-result>
  </div>
</template>

<script>
import SurveyContent from './components/SurveyContent.vue'
import UserExperince from './components/UserExperiences.vue'

export default {
  name: 'App',
  components: {
    appSurveyContent: SurveyContent,
    appSurveyResult: UserExperince,
  },
  beforeCreate() {
        document.body.background = 'red';
    }
}
</script>

<style>
body {
  background: linear-gradient(180deg,#dff9fb 0%, #dff9fb 100%);
}


::-webkit-scrollbar {
  display: none;
}

</style>

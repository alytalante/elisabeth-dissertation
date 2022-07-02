<template>
  <div class="video">VIDEO HERE</div>
  <div class="question">
    <strong>{{ questions[0] }}</strong>
    <p class="option">I pick the blue shirt</p>
    <p class="option">I pick the white shirt</p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      currentVideoUrl: null,
      currentStep: 1,
      questions: [
        "Do you want the blue shirt or the white shirt?",
        "Do you want to go to a bar or to go to a library?",
        "Do you want to ask Jonah why he pushed back on Brett or tell Jonah not to argue with Brett?",
        "Do you tell her what just happened with your brother or deflect?",
        "Do you think it’s better to commit a dangerous act for necessary change, or do you think violent change could be worse?",
        "Do you help them or try to talk them out of it?",
        "Do you cover for the radicals or turn them in?",
        "Do you join her or  let her go alone?",
        "Do you blow up the centre or walk away?",
      ],
      q1Answer: null,
      q2Answer: null,
      q3Answer: null,
      q4Answer: null,
      q5Answer: null,
      q6Answer: null,
      q7Answer: null,
      q8Answer: null,
      q9Answer: null,
    };
  },
  components: {},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.video {
  border: 1px solid black;
  width: 400px;
  margin: 20px auto;
  height: 240px;
}

.question {
  width: 400px;
  margin: 20px auto;
}

.option {
  cursor: pointer;
}
</style>

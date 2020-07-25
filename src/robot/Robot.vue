<template>
  <div>
    <label for="question">Please ask a yes or no question.</label>
    <input name="question" type="text" v-model="question" />
    <button>Search</button>

    <p>{{ answer }}</p>
    <img :src="image" alt="" v-if="image">
  </div>
</template>

<script>
import _ from "lodash";
import axios from "axios";

export default {
  data() {
    return {
      question: "",
      answer: "",
      image: ""
    };
  },
  watch: {
    question() {
      this.answer = "waiting for you to stop typing...";
      this.getAnswer();
    },
  },
  methods: {
    getAnswer: _.debounce(function() {
      if (this.question.indexOf("?") === -1) {
        this.answer = "Questions usually contain a question mark.";
        return;
      }

      this.answer = "Thinking...";

      axios
        .get("https://yesno.wtf/api")
        .then((res) => {
          this.answer = _.capitalize(res.data.answer)
          this.image = res.data.image
        })
        .catch((error) => {
          this.answer = "Error, could not reach the api: " + error;
        });
    }, 500),
  },
};
</script>

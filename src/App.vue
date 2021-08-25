<template>
  <div class="container">
    <div class="card">
      <div class="ratingSection">
        <span
          ><strong>Score :</strong> {{ score }} /
          {{ this.questions.length }}</span
        >
        <span
          ><strong>Questions :</strong> {{ currentIndex }} /
          {{ this.questions.length }}</span
        >
      </div>
      <div class="question">{{ questions[currentIndex].question }}</div>
      <div>
        <div
          class="label"
          :for="options"
          v-for="(options, index) in questions[currentIndex].ans"
          :key="index"
          :class="{
            'bg-red':
              index === answer &&
              index != questions[currentIndex]['correctOption'],
            'bg-green':
              index === questions[currentIndex]['correctOption'] &&
              answer != null,
          }"
        >
          <div @click="!answer ? setAnswer(index) : false">
            {{ options }}
          </div>
        </div>
      </div>
      <div class="center">
        <button class="button" @click="next()" :disabled="!answer">
          Next
        </button>
      </div>
    </div>

    <div class="modal-container" id="score-modal">
      <div class="modal-content-container">
        <h1>Congratulations, Quiz Completed.</h1>

        <div class="grade-details">
          <p>Attempts : 5</p>
          <p>
            Wrong Answers :
            <span id="wrong-answers">
              {{ wrongAnswer }}
            </span>
          </p>
          <p>
            Right Answers : <span id="right-answers"> {{ rightAnswer }}</span>
          </p>
          <p>
            Grade : <span id="grade-percentage">{{ percentage }} </span>%
          </p>
          <p>
            <span id="remarks" :class="remarkColor">{{ remark }}</span>
          </p>
        </div>

        <div class="modal-button-container">
          <button @click="closeModal()">Continue</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      currentIndex: 0,
      wrongAnswer: 0,
      rightAnswer: 0,
      percentage: 0,
      remarks: null,
      answer: null,
      remarkColor: null,
      reset: false,
      score: 0,
      questions: [
        {
          question:
            "We can pass original dom event into methods using ___ variable ?",
          ans: {
            optionA: "value",
            optionB: "$value",
            optionC: "$event",
            optionD: "$e",
          },
          correctOption: "optionC",
        },
        {
          question: "A static component can be cached by usnig ___ directive ?",
          ans: {
            optionA: "v-passive",
            optionB: "v-cached",
            optionC: "v-self",
            optionD: "v-once",
          },
          correctOption: "optionD",
        },
        {
          question: "We can aviod prop drilling in vuejs using ___ Api ?",
          ans: {
            optionA: "Context Api",
            optionB: "template refs",
            optionC: "provide/inject",
            optionD: "slots",
          },
          correctOption: "optionC",
        },
        {
          question: "Which guard supports the callback function ?",
          ans: {
            optionA: "beforeRouteEnter",
            optionB: "beforeRouteUpdate",
            optionC: "beforeRouteLeave",
            optionD: "beforeRouteEnter",
          },
          correctOption: "optionA",
        },
        {
          question: "Which history mode has bad impact on SEO ?",
          ans: {
            optionA: "HTML 5 Mode",
            optionB: "Hash Mode",
            optionC: "HTML ",
            optionD: "Router Mode",
          },
          correctOption: "optionB",
        },
      ],
    };
  },
  methods: {
    setAnswer(e) {
      this.answer = e;
      if (this.answer == this.questions[this.currentIndex]["correctOption"]) {
        this.score++;
        this.rightAnswer++;
      } else {
        this.wrongAnswer++;
      }
    },
    next() {
      if (this.currentIndex >= this.questions.length - 1) {
        this.percentage = (this.score / 10) * 100;
        if (this.score <= 1) {
          this.remark = "Bad Grades, Keep Practicing.";
          this.remarkColor = "red";
        } else if (this.score >= 2 && this.score < 4) {
          this.remark = "Average Grades, You can do better.";
          this.remarkColor = "orange";
        } else if (this.score >= 5) {
          this.remark = "Excellent, Keep the good work going.";
          this.remarkColor = "green";
        }
        document.getElementById("score-modal").style.display = "flex";
      } else {
        this.currentIndex++;
        this.answer = null;
      }
    },
    closeModal() {
      document.getElementById("score-modal").style.display = "none";
      this.currentIndex = 0;
      this.remarks = null;
      this.remarkColor = null;
      this.answer = null;
      this.percentage = 0;
      this.score = 0;
      this.rightAnswer = 0;
      this.wrongAnswer = 0;
    },
  },
};
</script>

<style>
body {
  background: #ccc;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.ratingSection {
  display: flex;
  margin-bottom: 1.5em;
  justify-content: space-between;
  font-weight: bold;
}
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
  height: 90vh;
}
.card {
  -webkit-box-shadow: 0 10px 6px -6px #777;
  -moz-box-shadow: 0 10px 6px -6px #777;
  box-shadow: 0 10px 6px -6px #777;
  transition: 0.3s;
  width: 35em;
  min-height: 20em;
  margin-inline: auto;
  box-sizing: border-box;
  padding: 2em;
  background: #fff;
  border-radius: 5px;
}

.question {
  box-sizing: border-box;
  height: 30%;
  font-weight: bold;
  width: 70%;
  border-radius: 10px;
  padding: 2em;
  border-radius: 5px;
  margin-inline: auto;
  box-shadow: 0 3px 10px rgb(0 0 0 / 0.2);
  text-align: center;
}
.hidden {
  display: none;
}

.label {
  display: block;
  width: 70%;
  border-radius: 5px;
  margin: auto;
  height: 2em;
  margin-block: 1em;
  box-sizing: border-box;
  padding: 5px;
  border: 1px solid;
  transition: 0.3s;
  background-color: #5a6268;
  color: white;
}
.label:hover {
  cursor: pointer;
}
.button {
  height: 30px;
  width: 100px;
  border: none;
  border-radius: 5px;
  color: white;
  font-weight: bold;
  background-color: #5a6268;
}
button:hover {
  box-shadow: 0 12px 16px 0 rgba(0, 0, 0, 0.24),
    0 17px 50px 0 rgba(0, 0, 0, 0.19);
}
.center {
  text-align: center;
}
.bg-green {
  background-color: green;
}
.bg-red {
  background-color: rgb(128, 0, 0);
}

.modal-container {
  display: none;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.4);
  flex-direction: column;
  align-items: center;
  justify-content: center;
  -webkit-animation: fadeIn 1.2s ease-in-out;
  animation: fadeIn 1.2s ease-in-out;
}

.modal-content-container {
  height: 25rem;
  width: 25rem;
  background-color: rgb(43, 42, 42);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  border-radius: 25px;
}

.modal-content-container h1 {
  font-size: 1.3rem;
  height: 3rem;
  color: lightgray;
  text-align: center;
}

.grade-details {
  width: 15rem;
  height: 10rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
}

.grade-details p {
  color: white;
  text-align: center;
}

.modal-button-container {
  height: 2rem;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-button-container button {
  width: 10rem;
  height: 2rem;
  background: none;
  outline: none;
  border: 1px solid rgb(252, 242, 241);
  color: white;
  font-size: 1.1rem;
  cursor: pointer;
  border-radius: 20px;
}
.modal-button-container button:hover {
  background-color: rgb(83, 82, 82);
}

.orange {
  color: orange;
}
.green {
  color: green;
}
.red {
  color: red;
}
</style>

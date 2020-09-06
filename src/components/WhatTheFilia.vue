<template>
<div class="WhatTheFilia">
    <div class="container-app">
      <div class="container-quiz">
        <div class="quiz-header">
          <h1>WTF</h1>
        </div>

        <div class="step-progress" :style="{'width': progress + '%'}"></div>

        <div class="quiz-main" v-for="(element,index) in questions.slice(a, b)" :key="index" v-show="quiz">
          <div class="box-question">
            <h2>
              What The Filia</h2>
            <p>{{element.question}}</p>
          </div>
          <div class="box-suggestions">
            <ul>
              <li v-for="(item,index) in element.suggestions" :key="index" :class="select ? check(item) : ''" @click="selectResponse(item)">{{item.suggestion}}</li>
            </ul>
          </div>
        </div>

        <div class="box-score" v-if="shown_score">
          <h2>Your score is</h2>
          <h3>{{score}}/{{questions.length}}</h3>
          <div class="btn-restart">
            <button @click="restartQuiz"> Restart <i class="fas fa-sync"></i></button>
          </div>
        </div>

        <div class="quiz-footer">
          <div class="box-button" v-if="progress < 100">
            <button @click="skipQuestion">Skip</button>
            <button @click="nextQuestion">Next</button>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WhatTheFilia',
  data() {
    return {
      questions:[
        {
          question:'A person who likes cats, a cat lover, is a...',
          suggestions:[
            {suggestion:'Ceraunophile'},
            {suggestion:'Dendrophile'},
            {suggestion:'Ailurophile', correct:true},
            {suggestion:'Cynophile'},
          ]
        },
        {
          question:'A person who loves lightning and thunder is a...',
          suggestions:[
            {suggestion:'Heliophile'},
            {suggestion:'Ceraunophile', correct:true},
            {suggestion:'Pluviophile'},
            {suggestion:'Chionophile'},
          ]
        },
        {
          question:'Someone who loves the sea, the ocean is a...',
          suggestions:[
            {suggestion:'Thalassophile', correct:true},
            {suggestion:'Selenophile'},
            {suggestion:'Oenophile'},
            {suggestion:'Limnophile'},
          ]
        },
      ],

      a: 0,
      b: 1,
      select: false,
      score: 0,
      quiz: true,
      shown_score: false,
      progress: 0,
    }
  },

  methods: {

    selectResponse(answer) {
      this.select = true;

      if(answer.correct) {
        this.score++;
      }
    },

    check(status) {
      if (status.correct) {
        return 'correct'
      }
      return 'incorrect'
    },

    nextQuestion() {
      this.progress = this.progress + (100/this.questions.length);

      if (this.questions.length -1 == this.a) {
        this.shown_score = true;
        this.quiz = false;
      }
      {
        this.a++;
        this.b++;
        this.select = false;
      }
    },

    skipQuestion() {

      this.progress = this.progress + (100/this.questions.length);

      if (this.questions.length -1 == this.a) {
        this.shown_score = true;
        this.quiz = false;
      }
      {
        this.a++;
        this.b++;
      }
    },

    restartQuiz() {
      Object.assign(this.$data, this.$options.data());
    }
  }
}
</script>


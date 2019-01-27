<template>
  <div id="app">
    <h1>Math game. Level {{ level + 1 }}</h1>
    <hr>
    
    <div class="progress">
      <div class="progress-bar" :style="progressStyles"></div>
    </div> 

    <div class="components">
      <transition name="flip" mode="out-in">
        <app-start-screen 
            v-if="state == 'start'"
            @onStart = "onStart"
        ></app-start-screen>
        <app-question 
            v-else-if="state == 'question'"
            @success = "onQuestionSuccess"
            @error = "onQuestionError"
            :settings="levels[level]"
        ></app-question>
        <app-message 
            v-else-if="state == 'message'"
            :type="message.type"
            :text="message.text"
            @onNext = "onNext"
        ></app-message>
        <app-result-screen 
            v-else-if="state == 'result'"
            :stats="stats"
            @repeat="onStart"
            @nextLevel="onNextLevel"
        ></app-result-screen>
        <div v-else>Unknown state</div>
      </transition>
    </div>
  </div>
</template>

<script>
import AppMessage from './components/Message.vue'
import AppQuestion from './components/Question.vue'
import AppStartScreen from './components/StartScreen.vue'
import AppResultScreen from './components/ResultScreen.vue'

export default {
  name: 'app',
  components: {
    AppMessage,
    AppQuestion,
    AppStartScreen,
    AppResultScreen
  },
  methods: {
    onStart(){
      this.state = 'question'
      this.stats.success = 0
      this.stats.error = 0
    },
    onQuestionSuccess(){
      this.state = 'message'
      this.message.text = 'Good job!'
      this.message.type = 'success'
      this.stats.success++
    },
    onQuestionError(msg){
      this.state = 'message'
      this.message.text = msg
      this.message.type = 'danger'
      this.stats.error++
    },
    onNext(){
      if(this.questDone < this.questMax){
        this.state = 'question'
      } else {
        this.state = 'result'
      }
    },
    onNextLevel(){
      if(this.level<this.levels.length-1)
      {
        this.level++
        this.onStart()
      } else {
        this.level = 0
        this.onStart()
      }
    }
  },
  computed: {
    questDone(){
      return this.stats.success + this.stats.error
    },
    progressStyles(){
      return {
        width: (this.questDone / this.questMax * 100) + '%'
      }
    }
  },
  data () {
    return {
      state: 'start',
      stats: {
        success: 0,
        error: 0
      },
      message: {
        type: '',
        text: ''
      },
      questMax: 3,
      level: 0,
      levels: [
        {
          from: 10,
          to: 40,
          range: 5,
          variants: 2
        },
        {
          from: 100,
          to: 200,
          range: 20,
          variants: 4
        },
        {
          from: 500,
          to: 900,
          range: 40,
          variants: 6
        }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 5rem;
}

//.flip-enter {}

.progress {
  margin: 0 3rem 0 3rem;
}

.progress-bar {
  transition: 1s;
}

.flip-enter-active {
  animation: flipInX 0.3s linear;
}

//.flip-leave {}

.flip-leave-active {
  animation: flipOutX 0.3s linear;
}

@keyframes flipInX {
  from{transform: rotateX(90deg);}
  to{transform: rotateX(0deg);}
}

@keyframes flipOutX {
  from{transform: rotateX(0deg);}
  to{transform: rotateX(90deg);}
}
</style>

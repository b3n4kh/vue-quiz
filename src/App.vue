<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
      :numAnswered="numAnswered"
      @catalogChanged="change_catalog"
    />

    <b-container class="bv-example-row">
      <b-row align-v="center">
        <b-col md="2" sm="1"></b-col>
        <b-col md="8" sm="6">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numAnswered: 0,
      numTotal: 0,
    }
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numAnswered++
    },
    shuffle(array) {
      let currentIndex = array.length,  randomIndex;
      while (currentIndex != 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;
        [array[currentIndex], array[randomIndex]] = [ array[randomIndex], array[currentIndex] ];
      }
      return array;
    },
    change_catalog(catalog) {
      this.get_questions(catalog);
    },
    get_questions(catalog) {
      console.log('Getting Catalog: ' + catalog)
      fetch('/' + catalog + '.json', {
        method: 'get'
      }).then((response) => {
          return response.json()
        }).then((jsonData) => {
          console.log('jsonData: ' + JSON.stringify(jsonData.results, null, "    "));
          this.questions = this.shuffle(jsonData.results)
          this.numTotal = this.questions.length
        })
    }
  },
  mounted: function() {
    this.get_questions('src')
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

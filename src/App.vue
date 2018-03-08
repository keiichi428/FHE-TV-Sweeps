<template>
<!-- <div id="app" class="container"> -->
<div id="app" class="">
  <h3 v-html="headline" class="container headline"></h3>
  <img v-if="model" src="static/img/hero.jpg" class="container responsive-img hide-on-small-only" :alt="model.inHeaderMessage" width="1280">
  <img v-if="model" src="static/img/hero-xs.jpg" class="container responsive-img hide-on-med-and-up" :alt="model.inHeaderMessage" width="640">
  <div class="container">
    <EntryButton></EntryButton>
  </div>
  <div class="container">
    <Prizes v-if="model" :prizes="model.prizes"></Prizes>
  </div>
  <div class="container">
    <EntryForm v-if="model" :model="model"></EntryForm>
  </div>
</div>
</template>

<script>
import axios from 'axios'
import SmoothScroll from 'smoothscroll'
import EntryButton from './components/EntryButton.vue'
import Prizes from './components/Prizes.vue'
import EntryForm from './components/EntryForm.vue'

export default {
  name: 'app',
  components: {
    EntryButton,
    Prizes,
    EntryForm
  },
  data () {
    return {
      // Headline text at the very top.
      model: undefined,
      headline: ''
    }
  },
  methods: {
    onContentsDataLoad (res) {
      this.model = res.data
      this.headline = this.model.headline
      this.formHeader = this.model.formHeader
    }
  },
  created () {
    axios.get('./static/contents.json')
      .then(this.onContentsDataLoad)
  }
}
</script>

<style >
@import url('https://fonts.googleapis.com/css?family=Oswald:400,500,600');

body{
  background: #232323;
}
h3.headline {
  font-size: 1.1rem;
  line-height: 2em;
  font-weight: 400;
  color: #d57c07;
  text-transform: uppercase;
}
h3.headline img{
  height: 1.20em;
  position: relative;
  top: .1em;
}

@media (min-width:601px) {
  h3.headline {
    font-size: 2.0rem;
  }
  .form-header{
    font-size: 1.75rem;
  }
}

@media (min-width:1201px) {
  h3.headline {
    font-size: 2.5rem;
  }
}

#app {
  font-family: 'Oswald', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>

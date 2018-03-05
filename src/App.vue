<template>
<div id="app" class="container">
  <h3>{{headline}}</h3>
  <img src="http://via.placeholder.com/1280x500" class="responsive-img">
  <Prizes v-if="model" :prizes="model.prizes"></Prizes>
  <p class="form-header">{{formHeader}}</p>
  <EntryForm v-if="model" :model="model"></EntryForm>
</div>
</template>

<script>
import axios from 'axios'
import HelloWorld from './components/HelloWorld.vue'
import Prizes from './components/Prizes.vue'
import EntryForm from './components/EntryForm.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    Prizes,
    EntryForm
  },
  data () {
    return {
      // Headline text at the very top.
      model: undefined,
      headline: '',
      formHeader: ''
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

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Mukta+Malar:300,400,600');
h3 {
  font-size: 1.75rem;
  font-weight: 100;
}

.form-header{
  font-size: 1.5rem;
  margin: 3rem 0;
}

#app {
  font-family: 'Mukta Malar', sans-serif;
  /* font-family: 'Avenir', Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>

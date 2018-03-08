<template lang="html">
<div class="col s12">
  <div v-if="prizes" class="row prizes">
    <div v-for="(prize,index) in prizes" v-bind:key="index" class="col s6 m4 xl3 prize">
      <div class="card">
        <a class="card-image" :href="parsePrizeURL(prize.link)" :title="prize.title.replace(/<\/?[^>]+(>|$)/g, ' ')" target="_blank">
          <img :src="parsePrizeURL(prize.thumb)" class="responsive-img"/>
        </a>
        <div class="card-content">
          <h6 class="prize-title" v-html="prize.title"></h6>
          <p class="left-align prize-description" v-html="prize.description"></p>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'Prizes',
  props: ['prizes'],
  methods:{
    parsePrizeURL(path){
      if(path.indexOf("//") > -1){
          return path
      }else{
        return './static/prize/' + path
      }
    }
  }
}
</script>

<style >
.prizes{
  display:flex;
  flex-direction: row;
  flex-wrap: wrap;
  margin-top: -2rem;
}
.row .col.prize{
  margin-left: 0 !important;
  margin-top: 2rem;
}
.card{
  height: 100%;
  margin-top: 0;
}
.card .card-content{
  padding: .5rem 1.25rem 0 1.25rem;
}
.prize-title{
  font-weight: 600;
  text-transform: uppercase;
  line-height: 1.21em;
}

.prize-description{
  color: #808080;
  text-align: left;
}

.card .card-content .prize-title p{
  font-weight: 500;
  font-size:.75em;
  color: #d57c07;
  text-transform: uppercase;
}


</style>

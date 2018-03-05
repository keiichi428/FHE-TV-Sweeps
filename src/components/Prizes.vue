<template lang="html">
<div class="col s12">
  <div v-if="prizes" class="row prizes">
    <div v-if="prizes" v-for="prize in prizes" v-bind:key="prize" class="col s6 m4 l3 prize">
      <div class="card">
        <a class="card-image" :href="prize.link" target="_blank">
          <img :src="parsePrizeURL(prize.thumb)" class="responsive-img"/>
        </a>
        <div class="card-content">
          <h6>{{prize.title}}</h6>
          <p class="left-align">{{prize.description}}</p>
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
        return 'static/prize/' + path
      }
    }
  }
}
</script>

<style lang="css" scoped>
.prizes{
  display:flex;
  flex-direction: row;
  flex-wrap: wrap;
  margin-top: -1rem;
}
.row .col.prize{
  margin-left: 0 !important;
  margin-top: 2rem;
}
.card{
  height: 100%;
}
.card .card-content{
  padding: .5rem 1.25rem 0 1.25rem;
  /* word-break: break-all; */
}

</style>

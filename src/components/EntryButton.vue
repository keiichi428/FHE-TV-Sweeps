<template lang="html">
  <div class="">
    <a href="#entry" class=" btn-entry" @click="onClick"><span>ENTER HERE</span></a>
  </div>
</template>

<script>
import SmoothScroll from 'smoothscroll'
export default {
  name: 'EntryButton',
  description: 'A dead simple scrollbutton.',
  methods: {
    onClick (e) {
      let entry = document.querySelector('#entry')
      if (entry) { SmoothScroll(entry) }
    }
  }
}
</script>

<style scoped>
.btn-entry{
  display: inline-block;
  position: relative;
  width: 277px;
  height: 66px;
  background-image:url(../img/btn-enter.png);
  background-position: bottom;
  background-size: 100% 200%;
  line-height:66px;
  font-size: 36px;
  font-weight: 900;
  color: #fff;
  transition: all .3s;
  transform: scale(.85);
}

.btn-entry:before{
  content: "";
  position: absolute;
  top: 0; left: 0;
  width: 100%; height: 100%;
  background-image:url(../img/btn-enter.png);
  background-position: top;
  background-size: 100% 200%;
  transition: all .3s;
}

.btn-entry:hover{
  color: #000;
}
.btn-entry:hover:before{
  opacity: 0;
}

.btn-entry span{
  position: absolute;
  top: -1px; left: 0;
  width: 100%; height: 100%;
}

@media (min-width:601px) {
.btn-entry{
transform: scale(1);
}

}
</style>

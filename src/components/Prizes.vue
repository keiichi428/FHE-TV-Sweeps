<template lang="html">
<div class="col s12">
  <div v-if="prizes" class="row prizes">
    <div v-for="(prize,index) in prizes" v-bind:key="index" class="col s6 m4 xl3 prize">
      <div class="card">
        <!-- <a class="card-image" :href="parsePrizeURL(prize.link)" :title="prize.title.replace(/<\/?[^>]+(>|$)/g, ' ')" target="_blank"> -->
        <!-- <a class="card-image" :data-href="prize.link" @click="openPrizeModal" :title="prize.title.replace(/<\/?[^>]+(>|$)/g, ' ')" target="_blank"> -->
        <a class="card-image" :data-href="prize.link" @click="openPrizeModal" :title="getLoTitle(prize.title)" target="_blank">
          <img :src="parsePrizeURL(prize.thumb)" class="responsive-img" :alt="getLoTitle(prize.title)"/>
        </a>
        <div class="card-content">
          <h6 class="prize-title" v-html="prize.title"></h6>
          <p class="left-align prize-description" v-html="prize.description"></p>
        </div>
      </div>
    </div>
  </div>

  <!-- Modal -->
  <div id="modal-prize" class="modal">
    <div class="modal-content">
      <div id="yt-player-wrapper"></div>
    </div>
  </div>
</div>
</template>

<script>
import M from 'materialize-css'
let
prizeModal,
prizeYtPlayer,
prizeYtPlayerDone = false,
prizeYtPlayerEmergentBlur = false,
isMobile

export default {
  name: 'Prizes',
  props: ['prizes'],
  data () {
    return {
      prizeModalYoutubeUrl: '',
      loTitle:''
    }
  },
  methods: {
    parsePrizeURL (path) {
      if (path.indexOf('//') > -1) {
        return path
      } else {
        return './static/prize/' + path
      }
    },

    getLoTitle(title){
      return title.replace(/<\/?[^>]+(>|$)/g, ' ').toLowerCase()
    },
    openPrizeModal (e) {
      let iframe, requestFullScreen
      // const requestFullScreen = iframe.requestFullScreen || iframe.mozRequestFullScreen || iframe.webkitRequestFullScreen;
      console.log(prizeYtPlayer);
      console.log(e.currentTarget.attributes['data-href'].value)
      if (prizeModal) {
        prizeModal.open()
        this.prizeModalYoutubeUrl = e.currentTarget.attributes['data-href'].value
        if (prizeYtPlayer) {
          prizeYtPlayer.loadVideoById(
            {
              videoId: this.prizeModalYoutubeUrl
            })
        } else {
          this.initYoutube(this.prizeModalYoutubeUrl)
        }
        prizeYtPlayerDone = false

        // if(isMobile === 1){
        //   iframe = this.$el.querySelector("#yt-player-wrapper")
        //   requestFullScreen = iframe.requestFullScreen || iframe.mozRequestFullScreen || iframe.webkitRequestFullScreen;
        //   if (requestFullScreen) {
        //     requestFullScreen.bind(iframe)();
        //   }
        // }
      }
    },
    closePrizeModal (e) {
      prizeModal.close()
    },


    onPrizeModalCloseStart () {
      if (prizeYtPlayer) { prizeYtPlayer.stopVideo() }
    },

    onPrizeModalCloseEnd () {
      if (prizeYtPlayer) {
        prizeYtPlayer.destroy()
        prizeYtPlayer = undefined
      }
    },

    onPrizeModalYtPlayerReady (event) {
      prizeYtPlayer.playVideo()
    },


    onPrizeModalYtPlayerStateChange (event) {
      if (event.data == YT.PlayerState.ENDED && !prizeYtPlayerDone) {
        this.closePrizeModal()
        prizeYtPlayerDone = true
      }
    },

    initYoutube (videoId) {
      let vm = this
      if (!window.onYouTubeIframeAPIReady) {
        var tag = document.createElement('script')
        tag.src = 'https://www.youtube.com/iframe_api'
        var firstScriptTag = document.getElementsByTagName('script')[0]
        firstScriptTag.parentNode.insertBefore(tag, firstScriptTag)

        window.onYouTubeIframeAPIReady = this.onYouTubeIframeAPIReady
      } else {
        this.onYouTubeIframeAPIReady()
      }
    },
    onYouTubeIframeAPIReady () {
      prizeYtPlayer = new YT.Player('yt-player-wrapper', {
        videoId: this.prizeModalYoutubeUrl,
        showinfo: 0,
        fs: isMobile,
        playsinline: isMobile === 1 ? 0 : 1,
        rel: 0,
        events: {
          'onReady': this.onPrizeModalYtPlayerReady,
          'onStateChange': this.onPrizeModalYtPlayerStateChange
        }
      })
    },
    onDocumentVisibilityChange () {
      if (document.hidden) {
        if (prizeYtPlayer && prizeYtPlayer.getPlayerState() === YT.PlayerState.PLAYING) {
          prizeYtPlayer.pauseVideo()
          prizeYtPlayerEmergentBlur = true
        }
      } else {
        if (prizeYtPlayer && prizeYtPlayerEmergentBlur) {
          prizeYtPlayer.playVideo()
        }
      }
    }
  },
  created () {
    // this.initYoutube()
  },
  mounted () {
    // Create and Set Modal
    prizeModal = M.Modal.init(this.$el.querySelector('#modal-prize'), {
      onCloseStart: this.onPrizeModalCloseStart,
      onCloseEnd: this.onPrizeModalCloseEnd
    })
    isMobile = navigator.userAgent.match(/(iPhone|Android)/) ? 1 : 0;
    document.addEventListener('visibilitychange', this.onDocumentVisibilityChange)
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
.card-image{
  cursor: pointer;
}

.modal{
  overflow: hidden;
  background: #000;
}
.modal .modal-content{
  padding: 0;
  padding-bottom: 56.25%;
  background: #000;
  overflow: hidden;
}
.modal-content>iframe{
  position: absolute;
  overflow: hidden;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

</style>

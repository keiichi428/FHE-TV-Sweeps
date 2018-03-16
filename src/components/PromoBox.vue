<template lang="html">
  <div class="row">
    <div class="col s12 ">
      <div id="date_strip">
        <a href="http://amzn.to/2nsQQzT" target="_blank" onclick="dataLayer.push({'ea':{'name':'purchase','action':'click','label':'amazon'},'event':'se'})">
          <img src="static/img/bones-collection-boxart.png" alt="The Bones Collectionwith seasons 1-12 Available now on DVD.">
        </a>
        <a id="buy_text_wrap" href="http://amzn.to/2nsQQzT" target="_blank" onclick="dataLayer.push({'ea':{'name':'purchase','action':'click','label':'amazon'},'event':'se'})" style="display: inline-block;">
          <p id="buy_text"><span>The Bones Collection</span><span>with seasons 1-12</span><span class="line3">available now on DVD.</span></p>
        </a>
      </div>

      <div class="buy_links">
        <a href="http://apple.co/2meTN5U" target="_blank" onclick="dataLayer.push({'ea':{'name':'purchase','action':'click','label':'itunes'},'event':'se'})">
          <img src="static/img/itunes.png"class="btn_itunes" alt="Get it on iTunes">
        </a>
        <a href="http://amzn.to/2lyIIZr" target="_blank" onclick="dataLayer.push({'ea':{'name':'purchase','action':'click','label':'amazon'},'event':'se'})">
          <img src="static/img/amazon.png" class="btn_amazon" alt="Amazon Video">
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PromoBox'
}
</script>

<style lang="css" scoped>

 #date_strip {
    width: 100%;
    max-width: 550px;
    padding-right: 10px;
    background: #f37600;
    color: #fff;
    margin-top: 80px;
    margin-bottom: 50px;
    margin-left: auto;
    margin-right: auto;
    min-height: 100px;
    font-family: 'Open Sans',sans-serif;
}
@media (max-width: 698px){
  #date_strip {
      min-height: 72px;
  }
}

@media (max-width: 480px){
  #date_strip {
    padding-right: 8px;
    padding-left: 8px;
    min-height: 48px;
    text-align: center;
    margin-top: 150px;
  }
}

#date_strip a, a.button {
    color: #fff;
}
#date_strip a, #social_links a, .button {
    text-decoration: none;
}
#main img {
    max-width: 100%;
    margin: 20px auto;
}
#date_strip img {
    float: left;
    margin-top: -40px;
    margin-left: -4px;
}

@media (max-width: 480px){
  #date_strip img {
      width: 250px;
      float: none;
      height: auto;
      margin-top: -115px;
      margin-bottom: 0;
      padding-bottom: 0;
  }
}

#date_strip #buy_text {
    font-size: 16px;
}
@media (max-width: 480px){
  #date_strip #buy_text {
      font-size: 16px;
  }
  #date_strip p {
      letter-spacing: 1px;
      font-size: 9px;
      padding-top: 0;
      padding-bottom: 12px;
      margin: 0;
  }
}


#date_strip p span {
    font-size: 26px;
    display: block;
}

#date_strip p, .button {
    text-transform: uppercase;
}

#date_strip #buy_text span {
    font-size: 16px;
}

#date_strip #buy_text .line3 {
    font-size: 16px;
}

.buy_links{
  letter-spacing: 40px;
  text-align: center;
}

@media (max-width: 698px){
  #date_strip #buy_text span,
  #date_strip #buy_text .line3{
      font-size: 12px;
  }
}
@media (max-width: 480px){
  .buy_links{
    letter-spacing: 10px;
  }
}


.buy_links img{
  display: inline-block;
  border-radius: 4px;
}
.btn_amazon{
  background: #fff;
  padding: 2px 5px 0px 5px;
}
</style>

<template>
  <div class="card-wrap" :class="{'current': card.current}" :style="card.position">
    <div class="card" :class="{'card_front':!card.turned, 'card_back':card.turned}" @click="handleCard()" :ref="'card'+card.id">
      <div class="front">
        <figure>
          <img :src="card.img" alt="">
          <figcaption>{{card.caption}}</figcaption>
        </figure>
      </div>
      <div class="back">
        <p>{{card.desc}}</p>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Card",
    data(){
      return {
        turned: false,
        style: {
          transform: "",
          left: "calc(50% - 160px)",
          top: "calc(50% - 180px)"
        },
        style2:{}
      }
    },
    props:{
      card:{
        type: Object,
        default:function () {
          return {
            img: '',
            caption:'',
            desc: ''
          }
        }
      },
      current: {
        type: Object,
        default:function () {
          return {}
        }
      }
    },
    methods: {
      handleCard() {
        if (this.card.current) {

          this.card.turned = !this.card.turned
          return
        }
        // 点击不是当前卡片
        //this.card.position = this.style
        this.$emit('clickOther', this.card)
      }
    }
  }
</script>

<style scoped>
  .card-wrap {
    position: absolute;
    perspective: 1800px;
    transition: all 1s;
  }
  .current {
    z-index: 1;
  }
  .card{
    height: 360px;
    width: 320px;
    position: relative;
    transform-style: preserve-3d;
    transform-origin: 0 50%;
    transition: transform .8s;
  }
  .card_back{
    transform: translate(320px) rotateY(-180deg);
  }
  .card_front{
    transform: translate(0px) rotateY(0deg);
  }
  .front,.back{
    width: 100%;
    height: 100%;
    position: absolute;
  }
  .back{
    transform: rotateY(180deg);
    background: wheat;
    backface-visibility: hidden;
  }
  .front{
    z-index: 1;
    box-sizing: border-box;
    padding: 40px;
    background: white;
  }
  figure{
    padding: 0;
    margin: 0;
  }
  figure img{
    width: 240px;
    height: 240px;
  }
  figure figcaption {
    font-size: 18px;
    font-weight: bold;
    text-align: center;
  }
</style>

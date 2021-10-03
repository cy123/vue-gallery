<template>
  <div class="gallery" ref="gallery">
    <Card v-for="(card, idx) of cards" :card="card" :key="idx" v-on:clickOther="handleClickOther"></Card>
    <nav>
      <span v-for="(item, idx) of navCards" :key="idx"
        :class="{'current':item.current, 'back': item.turned}"
        @click="handleNavClick(item)"
      >

      </span>
    </nav>
  </div>
</template>

<script>
  import Card from "../components/Card"
  const number = require('lodash/number')
  import "@/assets/icon.css"
  export default {
    name: "Gallery",
    data(){
      return {
        cards: [],
        current: {},
        centerStyle: {
          transform: "",
          left: "calc(50% - 160px)",
          top: "calc(50% - 180px)"
        },
        arr:[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16],
        parts: [
            {
              current: false,
              turned: false
            },
          {
            current: false,
            turned: false
          },
          {
            current: false,
            turned: false
          },{
            current: false,
            turned: false
          },
          {
            current: false,
            turned: false
          },
          {
            current: false,
            turned: false
          },
          {
            current: true,
            turned: false
          }
            ],
        back: false,
        navCards:[]
      }
    },
    components: {
      Card
    },

    created(){
      window.addEventListener('load', () => {
        this.sortCards()
      });
      this.initCards()
      this.setNavCards()
    },
    methods:{
      initCards(){
        let idx2 = number.random(0, this.arr.length - 1)
        this.cards = this.arr.map((n, idx) => {
          return {
            id:idx,
            img: `/imgs/${n}.jpg`,
            caption: 'hello gallery',
            desc: 'this is gallery desc',
            current: idx === idx2,
            turned: false,
            position: this.centerStyle
          }
        })
      },
      sortCards(){
        let part = Math.floor((this.cards.length-1) / 2)
        part += number.random(0, 1)
        this.cards.forEach((card, idx)=>{
          if(card.current) {
            return false
          }
          if(idx > part) {
            // 右
            this.$set(card, 'position', this.getRandomPosition('right'))
          } else {
            // 左
            this.$set(card, 'position', this.getRandomPosition('left'))
          }
        })
      },
      getRandomPosition(side = 'left'){
        if(side === 'left') {
          //let per = 320 / 50
          let top = number.random(160, -480)
          let left = number.random(-10, 30)
          // let leftPx = number.random(0,160)
          let deg = number.random(-180, 180)
          return  {
            left: `${left}%`,
            top: `calc(50% + ${top}px)`,
            transform: `rotate(${deg}deg)`
          }
        }
        // right side
        let top = number.random(160, -480)
        let left = number.random(60, 90)
        //let leftPx = number.random(160, 320)
        let deg = number.random(-180, 180)
        return {
          left: `${left}%`,
          top: `calc(50% + ${top}px)`,
          transform: `rotate(${deg}deg)`
        }
      },
      handleClickOther(card){

        this.resetCurrent()

        this.setCurrent(card)
        this.sortCards()
      },
      handleNavClick(item){
        if(item.current) {
          this.$set(item,'turned', !item.turned)
        } else {
          this.handleClickOther(item)
        }
      },
      resetCurrent(){
        let card = this.cards.find(card=>card.current===true)
        if(card){
          this.$set(card, 'current', false)

          if(card.turned) {
            this.$set(card, 'turned', false)
          }
        }
      },
      setCurrent(card){
        this.$set(card, 'position', this.centerStyle)
        this.$set(card, 'current', true)
      },
      setNavCards(){
        let current = this.cards.find(card => card.current === true)
        this.navCards.push(current)
        let temp =[]
        this.cards.forEach(card=>{
          if(card.current===false){
            temp.push(card)
          }
        })
        let i=0;
        while (this.navCards.length < 6) {
          let idx = number.random(0, temp.length-1)
          this.navCards.push(temp[idx])
          temp.splice(idx, 1)
          i++
          if(i>100){
            break
          }
        }
      }
    },
    mounted() {
    },
    updated() {
    }
  }
</script>

<style scoped>
.gallery {
  height: 600px;
  width: 100%;
  position: relative;
  background: gainsboro;
}
  .gallery nav {
    width: 100%;
    line-height: 30px;
    position: absolute;
    bottom: 30px;
    text-align: center;
    transition: all .5s;
  }
  .gallery nav span{
    display: inline-block;
    height: 30px;
    width: 30px;
    border-radius: 15px;
    background: #aaa;
    margin: 0 5px;
    transform: scale(.48);
    transition: all .6s;
  }

  span::after{
    content: "\e965";
    font-family: "icomoon";
    color: white;
    opacity: 0;
    font-size: 80%;
  }
  .gallery nav span.current{
    transform: scale(1);
  }
  span.current::after{
    opacity: 1;
  }
  .gallery nav span.back{
    transform: rotateY(180deg);
    background: #888;
  }
  /*.gallery nav span.current{*/
    /*transform: scale(1);*/
    /*background: #888;*/
  /*}*/
</style>

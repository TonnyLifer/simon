<template>
  <div class="container">
    <h1>Simon Game</h1>
    <div class="wrap">
      <div class="box redback" @click="clk(1)" :class="{activered : activeClass == 1}"></div>
      <div class="box blueback" @click="clk(2)" :class="{activeblue : activeClass == 2}"></div>
      <div class="box greenback" @click="clk(3)" :class="{activegreen : activeClass == 3}"></div>
      <div class="box yellowback" @click="clk(4)" :class="{activeyellow : activeClass == 4}"></div>
    </div>
    <div v-if="arr.length">
      Уровень: {{arr.length}}
    </div>
    <!-- <div class="game__start-btn" @click="startGame">{{ centerButton }}
        <span v-show="playing">{{ showScore }}</span>
    </div> -->
    <div>
      <button @click="Start">СТАРТ</button>
    </div>
    <div style="margin-top:5px">
      <button @click="Restart">Рестарт</button>
    </div>
    <div class="game__mode">
      <p>Режим</p>
      <button :class="{active: activeBtn === 'btn1'}" @click="gameMode('easy'); activeBtn = 'btn1'">Легко</button>
      <button :class="{active: activeBtn === 'btn2'}" @click="gameMode('normal'); activeBtn = 'btn2'">Нормально</button>
      <button :class="{active: activeBtn === 'btn3'}" @click="gameMode('hard'); activeBtn = 'btn3'">Тяжело</button>
  </div>
  </div>
</template>

<script>
import bum1 from "../assets/sounds/bum1.mp3"
import bum2 from "../assets/sounds/bum2.mp3"
import bum3 from "../assets/sounds/bum3.mp3"
import bum4 from "../assets/sounds/bum4.mp3"
import error from "../assets/sounds/error.mp3"
import aplodis from "../assets/sounds/aplodis.mp3"
export default {
  // components:{
  //   "bum1":bum1
  // },
  name: 'SimonGame',
  data() {
    return {
      activeClass: null,
      arr: [],
      counter: 0,
      chek:0,
      select:null,
      // gameMode: 'normal',
      activeBtn: 'btn2',
      mode: 1000,
      speed:500,
      sounds: {
        1: bum1,
        2: bum2,
        3: bum3,
        4: bum4,
        5: error,
        6: aplodis,
      }
    }
  },
  methods: {
    Restart(){
      this.chek=0
      this.arr=[]
      this.counter=0
      this.select=null
      this.activeClass = null

    },
    gameMode(req){
      if (req=='easy'){
        this.mode=1500
        this.speed=700
      }
      else if (req=='normal'){
        this.mode=1000
        this.speed=500
      }
      else{
        this.mode=400
        this.speed=300
      }
    },
    clk(data){
      this.activeClass = data
      this.select = data
      this.checkWinLose(data);
      var audio = new Audio(this.sounds[data]);
      audio.play();
      this.pool = setTimeout(() => {
        this.activeClass = null
      },250)
    },
    adt(){
      this.pool = setTimeout(() => {
        alert('Победа')
        var audio = new Audio(this.sounds[6]);
        audio.play();
        this.Restart()
      },700)
      
    },
    checkWinLose(){
      for(this.chek; this.chek <= this.arr.length;){
        console.log('lol '+ this.chek + ' '+ this.arr.length + ' '+ this.arr[this.chek]+ ' '+this.select)
        if (this.select == this.arr[this.chek]){
          console.log('успех')
          if(this.chek+1 == this.arr.length){
            console.log('win'+ this.chek)
            if(this.chek == 5){
              this.adt()
              break
            }
            else{
              this.Start()
            }
          }
          this.chek++
          break
        }
        else{
          var audio = new Audio(this.sounds[5]);
          audio.play();
          alert('Вы проиграли')
          this.Restart()
          break
        }
      }
    },
    test(){
        this.activeClass = this.arr[this.counter]
        this.counter++
        var audio = new Audio(this.sounds[this.activeClass]);
        audio.play();
        this.pool = setTimeout(() => {
          this.activeClass=null
      },this.speed)
        
    },
    Start(){
      this.arr.push(Math.floor(Math.random() * (4 - 1+ 1 ) + 1))
      this.polling = setInterval(() => {
        
        this.test()
        if (this.arr.length+1 == this.counter){
          clearInterval(this.polling)
          this.counter=0
          this.activeClass=null
          this.chek=0
        }
      },this.mode)
      
    }
  }
}
</script>
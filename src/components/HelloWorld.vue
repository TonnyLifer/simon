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
      },

      // centerButton: "START",
      // playing: false,
      // isClickable: false,
      // isWon: false,
      // isWrong: false,
      // // gameMode: 'normal',
      // // activeBtn: 'btn2',
      // score: 0,
      // sequence: [],
      // sequenceInterval: null,
      // playerSequence: [],
      // sounds: {
      //   1: "sounds/plip.mp3",
      //   2: "sounds/govilive.mp3",
      //   3: "sounds/error.mp3",
      //   4: "sounds/sswitch1.mp3",
      //   5: "sounds/haa.mp3",
      //   6: "sounds/good.mp3",
      // },
      // isLit: {
      //   1: false,
      //   2: false,
      //   3: false,
      //   4: false
      // }
    }
  },
  // computed: {
  //   showScore() {
  //     if (this.isWon) {
  //       return "Play Again?";
  //     }
  //     return "Score: " + this.score;
  //   }
  // },
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
    // playSound(tile) {
    //   if(this.sounds[tile]) {
    //     var audio = new Audio(this.sounds[tile]);
    //     audio.play();
    //   }
    // },
    // startGame() {
    //   this.playing = true;
    //   this.sequence = [];
    //   this.playSound(5);
    //   this.playerSequence = [];
    //   this.centerButton = "RESET";
    //   this.isWon = false;
    //   this.isWrong = false;
    //   this.score = 0;
    //   clearInterval(this.sequenceInterval);
    //   this.showSequence();
    // },
    // clicked(tile) {
    //   if (this.isClickable) {
    //     this.playSound(tile);
    //     this.lightUp(tile);
    //     this.playerSequence.push(tile);
    //     this.checkWinLose();
    //   }
    // },
    // checkWinLose() {
    //   // check for incorrect
    //   for (let i = 0; i < this.playerSequence.length; i++) {
    //     if (this.playerSequence[i] !== this.sequence[i]) {
    //       this.playerSequence = [];
    //       this.centerButton = "Wrong!";
    //       this.isWrong = true;
    //       setTimeout(() => {
    //         this.centerButton = "RESET";
    //         this.isWrong = false;
    //       }, 1000);
    //       this.showSequence(true);
    //     }
    //   }
    //   // if all correct and same length , continue
    //   if (this.playerSequence.length === this.sequence.length) {
    //     this.playerSequence = [];
    //     this.score++;
    //     // if win condition, show win, dont continue.
    //     if (this.score === 5) {
    //       this.centerButton = "Winner!";
    //       setTimeout(() => this.playSound(6), 600);
    //       this.isClickable = false;
    //       this.isWon = true;
    //     } else {
    //       this.showSequence();
    //     }
    //   }
    // },
    // lightUp(tile) {
    //   this.isLit[tile] = true;
    //   setTimeout(() => {
    //     this.isLit[tile] = false;
    //   }, 300);
    // },
    // showSequence(redo) {
    //   let currentIndex = 0;
    //   let speed = this.sequence.length === 0 ? 1000 : this.getModeSpeed(this.gameMode);
    //   this.isClickable = false;
    //   if (!redo) {
    //     // dont add number on incorrect answers
    //     this.sequence.push(Math.floor(Math.random() * 4 + 1));
    //   }
    //   this.sequenceInterval = setInterval(() => {
    //     if (currentIndex >= this.sequence.length) {
    //       clearInterval(this.sequenceInterval);
    //       return (this.isClickable = true);
    //     }
    //     this.playSound(this.sequence[currentIndex]);
    //     this.lightUp(this.sequence[currentIndex]);
    //     currentIndex++;
    //   }, speed);
    // },
    // getModeSpeed(mode) {
    //   if (mode === 'easy') {
    //     return 1500;
    //   }
    //   else if (mode === 'normal') {
    //     return 1000;
    //   }
    //   else {
    //     return 400;
    //   }
    // }
  }
}
</script>
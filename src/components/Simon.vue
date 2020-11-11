<template>
  <div class="row">
    <div class="simon">
      <button
        v-for="btn in btns"
        :id="btn.id"
        type="button"
        class="btn"
        :style="{
          background: btn.color,
          borderRadius: btn.borderRadius,
          opacity: btn.opacity,
        }"
        :key="btn.key"
        @click="input(btn.id)"
      ></button>
    </div>
    <div class="info">
      <h2>Раунд: {{ round }}</h2>
      <button v-if='!started'
        id="start" 
        type="button" 
        class="btn-start" 
        @click="start"
      >
        Играть
      </button>
      <button
        v-else 
        id="start" 
        type="button" 
        class="btn-start" 
        @click="stop"
      >
        Стоп
      </button>
      <p v-if="lose">Вы проиграли на {{ lose_round }} раунде</p>
    </div>
    <div class="option">
      <h2>Уровень сложности:</h2>
      <input
        type="radio"
        name="level"
        value="1500"
        checked
        v-model="difficulty"
      />
      <p>Легкий (время на нажатие - 1,5 сек)</p>
      <br />
      <input type="radio" name="level" value="1000" v-model="difficulty" />
      <p>Средний (время на нажатие - 1,0 сек)</p>
      <br />
      <input type="radio" name="level" value="400" v-model="difficulty" />
      <p>Сложный (время на нажатие - 0,4 сек)</p>
    </div>
    <div class="sound">
      <audio src="../assets/sounds/1.mp3" ref="1"></audio>
      <audio src="../assets/sounds/2.mp3" ref="2"></audio>
      <audio src="../assets/sounds/3.mp3" ref="3"></audio>
      <audio src="../assets/sounds/4.mp3" ref="4"></audio>
    </div>
  </div>
</template>

<script>
export default {
  name: "Simon",
  props: [],

  data() {
    return {
      started: false,
      round: 0,
      lose: false,
      win: false,
      lose_round: 0,
      btns: [
        {
          id: 1,
          color: "red",
          borderRadius: "125px 0 0 0",
          opacity: 0.5,
        },
        {
          id: 2,
          color: "blue",
          borderRadius: "0 125px 0 0",
          opacity: 0.5,
        },
        {
          id: 3,
          color: "yellow",
          borderRadius: "0 0 0 125px",
          opacity: 0.5,
        },
        {
          id: 4,
          color: "green",
          borderRadius: "0 0 125px 0",
          opacity: 0.5,
        },
      ],
      series: [],
      userInput: [],
      difficulty: 1500,
    };
  },

  methods: {
    start() {
      this.started = !this.started;
      this.lose = false;
      this.lose_round = 0;
      if (this.started) {
        this.addRound();
        this.playSeries();
      } else {
        this.stop();
      }
    },

    stop() {
      this.started = !this.started;
      this.lose = true;
      this.round = 0;
      this.series = [];
      this.userInput = [];
    },

    addRound() {
      this.round++;
      this.lose_round = this.round;
      this.series.push(this.random());
    },

    playSeries() {
      let timer = +this.difficulty;
      let self = this;
      this.series.forEach((item, i, ar) => {
        this.$refs[item].paused;
        this.$refs[item].currentTime = 0;
        if (i == ar.length - 1) {
          setTimeout(function () {
            self.playLightAudio(item);
          }, timer);
        } else {
          setTimeout(function () {
            self.playLightAudio(item);
          }, timer);
          timer += +this.difficulty;
        }
      });
    },

    playLightAudio(id) {
      let timer = +this.difficulty - 150;
      this.$refs[id].play();
      this.btns[id - 1].opacity = 1;
      setTimeout(this.defaultBtn, timer);
    },

    defaultBtn() {
      this.btns.forEach((i) => {
        i.opacity = 0.5;
      });
    },

    random() {
      return Math.floor(Math.random() * 4) + 1;
    },

    input(id) {
      if (this.started) {
        this.userInput.push(id);
        this.playLightAudio(id);
        if (
          this.userInput[this.userInput.length - 1] !=
          this.series[this.userInput.length - 1]
        ) {
          this.stop();
        } else {
          if (this.userInput.length == this.series.length) {
            let self = this;
            this.userInput = [];
            setTimeout(function () {
              self.addRound();
              self.playSeries();
            }, this.difficulty);
          }
        }
      }
    },

    test() {
      console.log(typeof +this.difficulty);
    },
  },
};
</script>

<style scoped>
h2 {
  font-size: 18px;
  text-align: center;
  margin: 20px 0;
}
.row {
  margin: 0 auto;
  width: 500px;
}
.simon {
  width: 250px;
  margin: 25px 30px 25px 0;
  float: left;
}
.btn {
  width: 125px;
  height: 125px;
  border: 0;
}
.btn-red {
  border-radius: 125px 0 0 0;
  background: red;
}
.btn-blue {
  background: blue;
  border-radius: 0 125px 0 0;
}
.btn-yellow {
  background: yellow;
  border-bottom-left-radius: 125px;
}
.btn-green {
  background: green;
  border-bottom-right-radius: 125px;
}
.btn-start {
  background: #00af0f;
  padding: 10px 30px;
  border-radius: 10px;
  border: 0;
  margin: 0 50px;
  text-transform: uppercase;
  font-weight: 600;
}
.btn-start:hover,
.btn-start:focus {
  background: #005f08;
  color: white;
}
.info p {
  text-align: center;
  color: #8c0000;
}
.option input {
  float: left;
}
.option p {
  margin: 0;
  font-size: 12px;
  color: gray;
}
.audio,
br {
  display: none;
}

.opacity {
  opacity: 1;
}
.non-opacity {
  opacity: 0.5;
}
</style>

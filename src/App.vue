<template>
  <div id="app">
    <div class="wrapper clearfix">
      <players
        :isWinner="isWinner"
        v-bind:scoresPlayer="scoresPlayer"
        v-bind:activePlayer="activePlayer"
        :currentScore="currentScore"
      />
      <controls
        :finalScore="finalScore"
        :isPlaying="isPlaying"
        @handleChangeFinalScore="handleChangeFinalScore"
        @handleNewGame="handleNewGame"
        @handleRollDice="handleRollDice"
        @handleHoldScore="handleHoldScore"
      />
      <dices :dices="dices" />
      <popup-rule @handleConfirm="handleConfirm" :isOpenPopup="isOpenPopup" />
    </div>
  </div>
</template>

<script>
import Players from "./components/Players.vue";
import Controls from "./components/Controls.vue";
import Dices from "./components/Dices.vue";
import PopupRule from "./components/PopupRule.vue";
export default {
  name: "app",
  data() {
    return {
      isPlaying: false,
      isOpenPopup: false,
      activePlayer: 0, //Ai là người chơi hiện tại
      scoresPlayer: [0, 0],
      dices: [1, 5],
      currentScore: 0,
      finalScore: 20,
    };
  },

  computed:{
    isWinner(){
      let {scoresPlayer, finalScore} = this;
      if(scoresPlayer[0]>=finalScore || scoresPlayer[1]>=finalScore){
        //Dừng chơi
        this.isPlaying = false;
        return true;
      }
      return false;
    }
  },

  methods: {
    handleNewGame() {
      this.isOpenPopup = true;
    },

    handleConfirm() {
      this.isPlaying = true;
      this.isOpenPopup = false;
      this.activePlayer = 0;
      this.scoresPlayer = [0, 0];
      this.dices = [1, 1];
      this.currentScore = 0;
    },

    nextPlayer() {
      //Đổi lượt chơi
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      //Reset điểm tạm thời về 0
      this.currentScore = 0;
    },

    handleRollDice() {
      if (this.isPlaying) {
        //Xoay xúc xắc
        //Math.random()=> 0->1
        let dice1 = Math.floor(Math.random() * 6) + 1;
        let dice2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [dice1, dice2];

        if (dice1 === 1 || dice2 === 1) {
          let currentPlayer = this.activePlayer;
          setTimeout(() => {
            alert(
              `Người chơi ${currentPlayer + 1} đã quay trúng số 1. Rất tiếc!`
            );
          }, 10);
          this.nextPlayer();
        } else {
          //Cộng dồn vào điểm tạm thời cho người đang chơi
          setTimeout(() => {
            this.currentScore += dice1 + dice2;
          }, 10);
        }
      } else {
        alert("Vui lòng click vào New Game");
      }
    },

    handleHoldScore() {
      if (this.isPlaying) {
        // let cloneScoresPlayer = [...this.scoresPlayer];
        // cloneScoresPlayer[this.activePlayer]+= this.currentScore;
        // this.scoresPlayer = cloneScoresPlayer;

        this.$set(
          this.scoresPlayer,
          this.activePlayer,
          this.scoresPlayer[this.activePlayer] + this.currentScore
        );

        if(!this.isWinner){
          this.nextPlayer();
        }
      } else {
        alert("Vui lòng click vào New Game");
      }
    },
    
    handleChangeFinalScore(e){
      this.finalScore = +e.target.value;
      console.log(this.finalScore);
    }
  },

  components: {
    Players,
    Controls,
    Dices,
    PopupRule,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(
      rgba(62, 20, 20, 0.4),
      rgba(62, 20, 20, 0.4)
    ),
    url("/public/assets/back.jpg");
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}
</style>

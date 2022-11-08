<template>
  <div
    id="game"
    class="container d-flex flex-column align-items-center p-5 w-md-75"
  >
    <header class="me-auto mb-4">
      <h1>Welcome to<br />MONSTER SLAYER Game</h1>
    </header>
    <character-info
      :monster-health="monsterHealth"
      :player-health="playerHealth"
    ></character-info>
    <game-over
      v-if="winner"
      :winner="winner"
      @start-again="restart"
    ></game-over>
    <all-button
      v-else
      :monster-health="monsterHealth"
      :player-health="playerHealth"
      @monster-hp="monsterHp"
      @player-hp="playerHp"
      @special-hp="specialHp"
      @heal-hp="healHp"
      @change-winner="changeWinner"
    ></all-button>

    <battle-log :log-msg="logMsg"></battle-log>
  </div>
</template>

<script setup>
import CharacterInfo from "./components/CharacterInfo.vue";
import AllButton from "./components/AllButton.vue";
import GameOver from "./components/GameOver.vue";
import BattleLog from "./components/BattleLog.vue";
import { reactive, ref, watch } from "vue";

let monsterHealth = ref(100);
let playerHealth = ref(100);
let winner = ref(null);
let logMsg = reactive({ arr: [] });

const restart = () => {
  playerHealth.value = 100;
  monsterHealth.value = 100;
  winner.value = null;
  logMsg.arr = [];
};
const addLogMsg = (who, what, val) => {
  logMsg.arr.unshift({
    actBy: who,
    actType: what,
    actVal: val,
  });
};

const monsterHp = (who, what, val) => {
  monsterHealth.value -= val;
  addLogMsg(who, what, val);
};
const playerHp = (who, what, val) => {
  playerHealth.value -= val;
  addLogMsg(who, what, val);
};
const specialHp = (who, what, val) => {
  monsterHealth.value -= val;
  addLogMsg(who, what, val);
};
const healHp = (who, what, val) => {
  if (playerHealth.value + val >= 100) {
    playerHealth.value = 100;
    //這裡看血條永遠不會滿100是因為 "先補完血 怪物才攻擊"
  } else {
    playerHealth.value += val;
  }
  addLogMsg(who, what, val);
};
const changeWinner = (who) => {
  winner.value = who;
};

watch(playerHealth, (val) => {
  if (val <= 0 && monsterHealth <= 0) {
    //draw
    changeWinner("draw");
  } else if (val <= 0) {
    //player lost
    changeWinner("monster");
  }
});
watch(monsterHealth, (val) => {
  if (val <= 0 && playerHealth <= 0) {
    //draw
    changeWinner("draw");
  } else if (val <= 0) {
    //monster lost
    changeWinner("player");
  }
});
</script>

<style>
* {
  font-family: "Rubik", sans-serif;
}
.sd {
  box-shadow: 0px 0px 17px rgba(0, 0, 0, 0.1);
}
.sd2 {
  box-shadow: 0px 0px 7px rgba(0, 0, 0, 0.3);
}
p {
  font-size: 15px;
  margin: 0;
}
.log--player {
  background-color: rgb(255, 193, 7);
  padding: 4px 10px;
  border-radius: 100px;
}
.log--monster {
  background-color: rgb(220, 53, 69);
  color: white;
  padding: 4px 10px;
  border-radius: 100px;
}
.log--heal {
  color: rgb(25, 135, 84);
  font-size: 1.25rem;
}
.log--damage {
  color: rgb(187, 45, 59);
  font-size: 1.25rem;
}
</style>

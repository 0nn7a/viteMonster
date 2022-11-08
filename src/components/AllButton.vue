<template>
  <!-- v-else -->
  <section class="my-4 row w-100 row-cols-2 row-cols-md-4">
    <div class="col pe-md-2 ps-0 pb-3">
      <button
        @click="attackMonster"
        class="btn btn-dark btn-lg p-3 sd2 w-100"
        style="height: 150px"
      >
        <i class="fa-2x mb-2 fa-solid fa-gun"></i><br />
        <p>ATTACK</p>
      </button>
    </div>
    <div class="col pe-md-2 pe-0">
      <button
        :disabled="mayUseSpecialAttack"
        @click="specialAttackMonster"
        class="btn btn-dark btn-lg p-3 sd2 w-100"
        style="height: 150px"
      >
        <i class="fa-2x fa-solid fa-skull mb-2"></i><br />
        <p>SPECIAL ATTACK</p>
      </button>
    </div>
    <div class="col ps-md-2 ps-0">
      <button
        :disabled="mayUseHeal"
        @click="healPlayer"
        class="btn btn-success btn-lg p-3 sd2 w-100"
        style="height: 150px"
      >
        <i class="fa-2x mb-2 fa-solid fa-briefcase-medical"></i><br />
        <p>HEAL</p>
      </button>
    </div>
    <div class="col ps-md-2 pe-0">
      <button
        @click="surrender"
        class="btn btn-danger btn-lg p-3 sd2 w-100"
        style="height: 150px"
      >
        <i class="fa-2x mb-2 fa-solid fa-flag"></i><br />
        <p>SURRENDER</p>
      </button>
    </div>
  </section>
</template>

<script>
//一些原生JSCode 不需要使用到Vue 可以直接定義在外部的JS環境即可
function getRandomVal(min, max) {
  return Math.floor(Math.random() * (max - min)) + min;
  //假如：希望得到值的區間為 5 - 12：random結果可能是0 乘多少都是0 那麼加5可固定最小值有5
  //random結果也可能無限趨近1 乘上最大值-最小值頂多無限趨近7 再加上5 也只會得到12以內的值
}
</script>

<script setup>
import { computed } from "@vue/reactivity";
import { ref } from "vue";

const props = defineProps({
  monsterHealth: Number,
  playerHealth: Number,
});
const emit = defineEmits([
  "monsterHp",
  "playerHp",
  "specialHp",
  "healHp",
  "changeWinner",
]);

let currentRound = ref(0);
const mayUseSpecialAttack = computed({
  get() {
    return currentRound.value % 3 !== 0;
    //設定強力攻擊只能3回合使用一次
  },
});
const mayUseHeal = computed({
  get() {
    return props.playerHealth === 100;
    //設定一開始滿血時無法補血
  },
});

const attackMonster = () => {
  if (currentRound.value !== 0 && currentRound.value !== 3) {
    currentRound.value++;
    //等強力攻擊使用過才開始計算是否過了3回合 避免第一擊使用普通攻擊而強力攻擊會有2回合無法使用
  }
  const attackVal = getRandomVal(5, 12);
  emit("monsterHp", "player", "attack", attackVal);
  attackPlayer();
};
const attackPlayer = () => {
  const attackVal = getRandomVal(8, 15);
  setTimeout(() => {
    emit("playerHp", "monster", "attack", attackVal);
  }, 200);
};
const specialAttackMonster = () => {
  currentRound.value = 1;
  const attackVal = getRandomVal(10, 25);
  emit("specialHp", "player", "sp", attackVal);
  attackPlayer();
};
const healPlayer = () => {
  if (currentRound.value !== 0 && currentRound.value !== 3) {
    currentRound.value++;
  }
  const healVal = getRandomVal(8, 20);
  emit("healHp", "player", "heal", healVal);
  attackPlayer();
};
const surrender = () => {
  emit("changeWinner", "monster");
};
</script>

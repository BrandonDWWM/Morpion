<template>
  <div class="main">
    <div class="contain">
    <div class="morpion">
      <div class="block1">
        <div class="carré" @click="move(0)"
        @keydown="handleKeydown($event, 0)" :tabindex="0">{{ carres[0] }}</div>
        <div class="carré" @click="move(1)"
        @keydown="handleKeydown($event, 1)" :tabindex="0">{{ carres[1] }}</div>
        <div class="carré" @click="move(2)"
        @keydown="handleKeydown($event, 2)" :tabindex="0">{{ carres[2] }}</div>
      </div>
      <div class="block2">
        <div class="carré" @click="move(3)"
        @keydown="handleKeydown($event, 3)" :tabindex="0">{{ carres[3] }}</div>
        <div class="carré" @click="move(4)"
        @keydown="handleKeydown($event, 4)" :tabindex="0">{{ carres[4] }}</div>
        <div class="carré" @click="move(5)"
        @keydown="handleKeydown($event, 5)" :tabindex="0">{{ carres[5] }}</div>
      </div>
      <div class="block3">
        <div class="carré" @click="move(6)"
        @keydown="handleKeydown($event, 6)" :tabindex="0">{{ carres[6] }}</div>
        <div class="carré" @click="move(7)"
        @keydown="handleKeydown($event, 7)" :tabindex="0">{{ carres[7] }}</div>
        <div class="carré" @click="move(8)"
        @keydown="handleKeydown($event, 8)" :tabindex="0">{{ carres[8] }}</div>
      </div>
    </div>
    <div class="bot">
        <p v-if="winner">{{ winner }} a gagné !</p>
        <p v-else-if="draw">Égalité !</p>
        <button @click="reset">Recommence</button>
      </div>
  </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';

const carres = ref(Array(9).fill(null));
const player = ref('X');
const winner = ref<string | null>(null);
const draw = ref(false);
const combinaisons = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
];

const win = () => {
  combinaisons.forEach((combination) => {
    const [a, b, c] = combination;
    if (carres.value[a] && carres.value[a] === carres.value[b]
      && carres.value[a] === carres.value[c]) {
      winner.value = carres.value[a];
    }
  });
  if (!carres.value.includes(null) && !winner.value) {
    draw.value = true;
  }
};

const move = (index: number) => {
  if (!carres.value[index] && !winner.value) {
    carres.value[index] = player.value;
    win();
    player.value = player.value === 'X' ? 'O' : 'X';
  }
};

const reset = () => {
  carres.value = Array(9).fill(null);
  player.value = 'X';
  winner.value = null;
  draw.value = false;
};

const handleKeydown = (event: KeyboardEvent, index: number) => {
  if (event.key === 'Enter' || event.key === ' ') {
    move(index);
  }
};
</script>

<style scoped lang="scss">
.main {
  width: 100vw;
  height: 40vh;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0;
  padding: 0;
}

.morpion {
  width: 40vw;
  border: 1px solid black;
  display: flex;
  flex-direction: column;
  background-color: orangered;
}

.carré {
  width: 100px;
  height: 100px;
  border: 1px solid black;
  margin: 5px;
  background-color: white;
}

.block1, .block2, .block3 {
  display: flex;
  width: 100%;
  justify-content: center;
}

.bot {
  margin: 10px;
  width: 40vw;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>

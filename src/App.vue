<script setup lang="ts">
import { ref } from "vue";
import { Player } from "./models/Player";
import CreatePlayer from "./components/CreatePlayer.vue";
import TicTac from "./components/TicTac.vue";

let players = ref<Player[]>([]);

let playerOne = ref<Player>(new Player("", ""));
let playerTwo = ref<Player>(new Player("", ""));

let gameNotStarted = ref(true);

const startGame = (playerOneText: string, playerTwoText: string) => {
  playerOne.value = new Player(playerOneText, "X");
  playerTwo.value = new Player(playerTwoText, "O");
  players.value.push(playerOne.value);
  players.value.push(playerTwo.value);
  console.log(players.value);

  gameNotStarted.value = false;
};
</script>

<template>
  <CreatePlayer @start-game="startGame" v-if="gameNotStarted"></CreatePlayer>
  <TicTac :users="players" @start-game="startGame" v-else></TicTac>
</template>

<script scoped></script>

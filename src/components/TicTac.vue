<script setup lang="ts">
import { reactive, ref } from "vue";
import { Player } from "../models/Player";
import { IGame } from "../models/IGame";

const winningUser = ref("");
const squareSymbol = ref("");
const playable = ref(true);
const gameBoard = ref(true);
const theWinnerText = ref("");
const theWinner = ref("");

let gameState = ref<IGame[]>([
  new IGame("", false, 0),
  new IGame("", false, 1),
  new IGame("", false, 2),
  new IGame("", false, 3),
  new IGame("", false, 4),
  new IGame("", false, 5),
  new IGame("", false, 6),
  new IGame("", false, 7),
  new IGame("", false, 8),
]);

interface IPlayerProps {
  users: Player[];
}

function restart() {
  console.log(gameState.value);
  gameState.value = [
    new IGame("", false, 0),
    new IGame("", false, 1),
    new IGame("", false, 2),
    new IGame("", false, 3),
    new IGame("", false, 4),
    new IGame("", false, 5),
    new IGame("", false, 6),
    new IGame("", false, 7),
    new IGame("", false, 8),
  ];
  playable.value = true;
  theWinnerText.value = "";
  theWinner.value = "";
  gameBoard.value = false;
}
function newGame() {
  gameBoard.value = true;
}
const props = defineProps<IPlayerProps>();
defineEmits(["startGame"]);

let currentPlayer = reactive(props.users[0]);

const chosenSquare = (index: number) => {
  if (playable.value) {
    if (currentPlayer.role == "X") {
      squareSymbol.value = "X";
      gameState.value[index].userRole = "X";
      gameState.value[index].played = true;
      winner(currentPlayer);
      currentPlayer = props.users[1];
    } else {
      squareSymbol.value = "O";
      gameState.value[index].userRole = "O";
      gameState.value[index].played = true;
      winner(currentPlayer);
      currentPlayer = props.users[0];
    }
  } else {
    console.log("Game Over");
  }

  // console.log(currentPlayer);
  //console.log(gameState.value);
};

function winner(currentPlayer: Player) {
  const winnerCombinations = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let i = 0; i < winnerCombinations.length; i++) {
    let isWinner = true;

    for (let j = 0; j < winnerCombinations[j].length; j++) {
      if (
        gameState.value[winnerCombinations[i][j]].userRole !==
        currentPlayer.role
      ) {
        isWinner = false;
      }
    }
    if (isWinner) {
      playable.value = false;
      winningUser.value = currentPlayer.username + "!";
      theWinnerText.value = "The Winner is";
      theWinner.value = winningUser.value;
    }
  }

  let playedWithoutWinning = 0;

  for (let i = 0; i < 9; i++) {
    if (gameState.value[i].played === true) {
      playedWithoutWinning++;
    }
  }
  if (playedWithoutWinning === 9) {
    console.log("oavgjort");
  }
}
</script>
<template>
  <div v-if="gameBoard">
    <h1>Welcome Lets Play!</h1>

    <p>X: {{ props.users[0].username }}</p>

    <p>O: {{ props.users[1].username }}</p>
    <h2>{{ theWinnerText }} {{ theWinner }}</h2>
    <div class="grid-container">
      <div
        class="square"
        v-for="(square, index) in gameState"
        :key="index"
        @click.once="chosenSquare(index)"
      >
        <p class="squareSymbol">{{ gameState[index].userRole }}</p>
      </div>
    </div>
    <button @click="restart">Restart</button>
  </div>
  <div v-else>
    <h2>{{ theWinnerText }} {{ theWinner }}</h2>
    <button @click="newGame">New Game</button>
  </div>
</template>

<style scoped>
.grid-container {
  display: grid;
  grid-template-columns: auto auto auto;
  column-gap: 10px;
  row-gap: 10px;
}

.square {
  display: flex;
  align-items: center;
  justify-content: center;
  /* background-color: orange; */
  border: rgb(215, 44, 110) 1px solid;
  height: 150px;
  width: 150px;
}

.squareSymbol {
  font-size: 130px;
  text-shadow: 8px 7px 20px rgb(215, 44, 110);
}
</style>

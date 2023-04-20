<script setup lang="ts">
let playerOneText = "";
let playerTwoText = "";

defineEmits(["startGame"]);
</script>

<template>
  <h1>Create Players!</h1>
  <div class="formContainer">
    <form
      class="create-player"
      @submit.prevent="
        () => {
          $emit('startGame', playerOneText, playerTwoText);
        }
      "
    >
      <label for="playerOne">Player 1</label>
      <input type="text" v-model="playerOneText" />
      <label for="playerTwo">Player 2</label>
      <input type="text" v-model="playerTwoText" />
      <button class="start-button">Start</button>
    </form>
  </div>
</template>

<style scoped>
.create-player {
  display: flex;
  flex-direction: column;
}

.start-button {
  margin-top: 30px;
  max-width: 100px;
  margin-left: 120px;
}
</style>

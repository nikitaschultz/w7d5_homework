<template lang="html">
  <div class="menu">
    <h2>How to Play</h2>
    <h3>The Object of the Game</h3>
    <p>To stack the cards back up into a single pile.</p>
    <h3>The Rules</h3>
    <p>Any card can replace the card to its left or the third card to its left provided they match in either suit or value.</p>
    <h3>Cards remaining: {{ cardsRemaining }}</h3>
    <div class="buttons">
      <button v-on:click="handleUndo" type="button" name="Undo" class="button">Undo</button>
      <button v-on:click="handleRestart" type="button" name="Restart" class="button">Restart</button><br>
    </div>
    <div class="cards-remaining">
      <card-item v-if="removedCards" :card="removedCards[0]"></card-item>
      <card-item v-if="removedCards" :card="removedCards[1]"></card-item>
      <card-item v-if="removedCards" :card="removedCards[2]"></card-item>
    </div>
  </div>
</template>

<script>
import CardItem from './CardItem.vue';
import { eventBus } from '../main.js';

export default {
  name: "game-menu",
  props: ["cardsRemaining", "removedCards"],
  components: {
    "card-item": CardItem
  },
  methods: {
    handleUndo(){
      eventBus.$emit('undo')
    },
    handleRestart(){
      location.reload()
    }
  }
}
</script>

<style lang="css" scoped>

  .menu {
    background-color: #8bbabb;
    width: 24%;
    padding: 2%;
    margin: 0px 1%;
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: 'MuseoModerno', cursive;
  }

  .cards-remaining {
    display: flex;
  }

  h2, h3 {
    margin: 2px;
    color: #464159;
  }

  .button {
    border: solid 1px #464159;
    border-radius: 8px;
    color: #464159;
    font-family: 'MuseoModerno', cursive;
    background-color: ghostwhite;
    height: 30px;
    width: 80px;
    margin: 5px 5px 0px 5px;
  }

  .buttons {
    display: flex;
    justify-content: space-between;
  }

  button:hover {
    border: solid 1px white;
    border-radius: 8px;
    color: white;
    background: #464159;
    font-family: 'MuseoModerno', cursive;
    height: 30px;
    width: 80px;
    margin: 5px 5px 0px 5px;
  }

</style>

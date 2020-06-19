<template lang="html">
  <div class="page-content">
    <br>
    <h1>Let's Play</h1>
    <h2>Accordion Solitaire</h2><br>
    <div class="game-content">
      <cards-in-play :cardsInPlay="cardsInPlay"></cards-in-play>
      <game-menu :cardsRemaining="cardsRemaining"></game-menu>
  </div>
    </div>
</template>

<script>
import CardsInPlay from './components/CardsInPlay.vue';
import GameMenu from './components/GameMenu.vue';

export default {
  name: "app",
  data(){
    return {
      cardsInPlay: null,
      selectedCard: null,
      cardsRemaining: null
    }
  },
  mounted(){
    fetch('https://deckofcardsapi.com/api/deck/new/draw/?count=52')
    .then(response => response.json())
    .then(data => this.cardsInPlay = data.cards)
  },
  components: {
    "cards-in-play": CardsInPlay,
    "game-menu": GameMenu
  }
}
</script>


<style lang="css" scoped>

 .page-content {
   display: flex;
   align-items: center;
   flex-direction: column;
   background-color: ghostwhite;
 }

 .game-content {
   display: flex;
 }

  h1, h2 {
    margin: 5px;
  }

</style>

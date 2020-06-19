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
import { eventBus } from './main.js';
import CardsInPlay from './components/CardsInPlay.vue';
import GameMenu from './components/GameMenu.vue';

export default {
  name: "app",
  data(){
    return {
      cardsInPlay: null,
      selectedCard: null
    }
  },
  mounted(){
    fetch('https://deckofcardsapi.com/api/deck/new/draw/?count=52')
    .then(response => response.json())
    .then((data) => {
      data.cards.forEach((card) => {
        card.selected = false;
      })
      this.cardsInPlay = data.cards
    })

    eventBus.$on('card-selected', (card) => {
      if(this.selectedCard){
        if(this.attemptMove(this.selectedCard, card)){
          console.log("GOOD MOVE!");
          this.selectedCard.selected = false;
          card.selected = true;
          this.selectedCard = card;
        }else{
          this.selectedCard.selected = false;
          card.selected = true;
          this.selectedCard = card;
        }
      }else{
        card.selected = true;
        this.selectedCard = card;
      }
    })
  },
  components: {
    "cards-in-play": CardsInPlay,
    "game-menu": GameMenu
  },
  methods: {
    attemptMove(selectedCard, moveCard){
      let validValue = false;

      if(selectedCard.value === moveCard.value){
        validValue = true;
      }else if(selectedCard.suit === moveCard.suit){
        validValue = true;
      }

      let validPosition = false;
      const selectedPosition = this.cardsInPlay.indexOf(selectedCard);
      const movePosition = this.cardsInPlay.indexOf(moveCard);

      if(selectedPosition - movePosition === 1){
        validPosition = true;
      }else if(selectedPosition - movePosition === 3){
        validPosition = true;
      }

      return validValue && validPosition
    }
  },
  computed: {
    cardsRemaining(){
      let value = 0
      if(this.cardsInPlay){
        value = this.cardsInPlay.length - 1
      }
      return value;
    }
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

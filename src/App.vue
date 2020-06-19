<template lang="html">
  <div class="page-content">
    <br>
    <h1>Let's Play</h1>
    <h2>Accordion Solitaire</h2><br>
    <div class="game-content">
      <cards-in-play :cardsInPlay="cardsInPlay"></cards-in-play>
      <game-menu :cardsRemaining="cardsRemaining" :removedCards="removedCards"></game-menu>
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
      selectedCard: null,
      removedCards: [],
      lastMoves: []
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
          this.selectedCard.selected = false;
          card.selected = false;
          this.removedCards.unshift(card);
          const removedIndex = this.cardsInPlay.indexOf(card);
          const selectedIndex = this.cardsInPlay.indexOf(this.selectedCard);
          this.lastMoves.unshift([selectedIndex, removedIndex]);
          this.cardsInPlay[removedIndex] = this.selectedCard;
          this.cardsInPlay.splice(selectedIndex, 1);
          this.deselectPotentialCards()
          this.selectedCard = null;
        }else{
          this.deselectPotentialCards()
          this.selectedCard.selected = false;
          card.selected = true;
          this.selectedCard = card;
          this.selectPotentialCards()
        }
      }else{
        this.deselectPotentialCards()
        card.selected = true;
        this.selectedCard = card;
        this.selectPotentialCards()
      }
    })

    eventBus.$on('undo', () => {
      const lastCard = this.removedCards.splice(0,1);
      const lastMove = this.lastMoves.splice(0,1);
      this.cardsInPlay.splice(lastMove[0][0], 0, this.cardsInPlay[lastMove[0][1]]);
      this.cardsInPlay.splice(lastMove[0][1], 1, lastCard[0]);
    })
  },
  components: {
    "cards-in-play": CardsInPlay,
    "game-menu": GameMenu
  },
  methods: {
    attemptMove(selectedCard, moveCard){
      let validValue = this.checkValue(selectedCard, moveCard);
      let validPosition = this.checkPosition(selectedCard, moveCard);
      return validValue && validPosition
    },
    checkValue(card1, card2){
      return (card1.value === card2.value || card1.suit === card2.suit);
    },
    checkPosition(selected, move){
      const selectedPosition = this.cardsInPlay.indexOf(selected);
      const movePosition = this.cardsInPlay.indexOf(move);
      const diff = selectedPosition - movePosition;
      return (diff === 1 || diff === 3)
    },
    selectPotentialCards(){
      if((this.potentialCard1 != null && this.checkValue(this.selectedCard, this.potentialCard1) === true)){
        this.potentialCard1.selected = true;
      }
      if((this.potentialCard2 != null && this.checkValue(this.selectedCard, this.potentialCard2) === true)){
        this.potentialCard2.selected = true;
      }
    },
    deselectPotentialCards(){
      if(this.potentialCard1){
        this.potentialCard1.selected = false;
      }
      if(this.potentialCard2){
        this.potentialCard2.selected = false;
      }
    }
  },
  computed: {
    cardsRemaining(){
      let value = 0
      if(this.cardsInPlay){
        value = this.cardsInPlay.length - 1
      }
      return value;
    },
    potentialCard1(){
      if(this.selectedCard){
        const selectedIndex = this.cardsInPlay.indexOf(this.selectedCard);
        const potentialIndex = selectedIndex - 1;
        if(potentialIndex > -1){
          return this.cardsInPlay[potentialIndex];
        }
      }
    },
    potentialCard2(){
      if(this.selectedCard){
        const selectedIndex = this.cardsInPlay.indexOf(this.selectedCard);
        const potentialIndex = selectedIndex - 3;
        if(potentialIndex > -1){
          return this.cardsInPlay[potentialIndex];
        }
      }
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
   font-family: 'MuseoModerno', cursive;
 }

 .game-content {
   display: flex;
 }

  h1, h2 {
    margin: 5px;
    color: #034f84;
  }

</style>

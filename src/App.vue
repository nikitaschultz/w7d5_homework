<template lang="html">
  <div class="page-content">
    <h1>Let's Play</h1>
    <h2>Accordion Solitaire</h2>
  </div>
</template>

<script>
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
    .then(data => this.cardsInPlay = data.cards)
  }
}
</script>

<style lang="css" scoped>
</style>

<template lang="html">
  <div id="cards">
    <card-item v-if="cardsInPlay" v-for="(card, index) in cardsInPlay" :card="card" :key="index"></card-item>
  </div>
</template>

<script>
import CardItem from './CardItem.vue';

export default {
  name: "cards-in-play",
  props: ['cardsInPlay'],
  components: {
    "card-item": CardItem
  }
}
</script>

<style lang="css" scoped>

  #cards {
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    justify-content: flex-start;
    width: 68%;
    padding: 1%;
    background-color: #6c7b95;
  }

</style>

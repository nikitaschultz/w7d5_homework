<template lang="html">
  <div v-if="card" v-on:click="handleCardSelect" class="card">
    <img id="card-image" :class="cardSelected" :src="card.image" alt="card_image" width="70px">
  </div>
</template>

<script>
import { eventBus } from '../main.js';

export default {
  name: "card",
  props: ["card"],
  methods: {
    handleCardSelect(){
      eventBus.$emit('card-selected', this.card)
    }
  },
  computed: {
    cardSelected(){
      return this.card.selected ? "selected" : "not-selected"
    }
  }
}
</script>

<style lang="css" scoped>

  .card {
    margin: 5px;
  }

  #card-image:hover {
    box-shadow: 0px 0px 7px 3px lightgoldenrodyellow;
  }

  .selected {
    box-shadow: 0px 0px 7px 3px lightgoldenrodyellow;
  }

  .not-selected {
    box-shadow: none;
  }

</style>

<template lang="html">
  <div class="menu">
    <h2>How to Play</h2>
    <h3>The Object of the Game</h3>
    <p>To stack the cards back up into a single pile.</p>
    <h3>The Rules</h3>
    <p>Any pile can be moved onto the pile to its left or the third pile to its left provided the top cards fo the piles match in either suit or value.</p>
    <h3>Cards remaining: {{ cardsRemaining }}</h3>
  </div>
</template>

<script>
export default {
  name: "game-menu",
  props: ["cardsRemaining"]
}
</script>

<style lang="css" scoped>

  .menu {
    background-color: lightblue;
    width: 24%;
    padding: 2%;
    margin: 0px 1%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

</style>

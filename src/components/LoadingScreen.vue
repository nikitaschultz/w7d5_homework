<template lang="html">
  <div class="loading-screen">
    <h2>Shuffling deck...</h2>
  </div>
</template>

<script>
export default {
  name: "loading-screen"
}
</script>

<style lang="css" scoped>

  h2 {
    color: #fcbf1e;
  }

  .loading-screen {
    display: flex;
    align-content: center;
    justify-content: center;
    width: 68%;
    padding: 1%;
    background-color: #6c7b95;
  }

</style>

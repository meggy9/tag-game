<template>
    <div class="playground">
      <div class="td" :class="[!td ? 'empty-td' : '']" v-for="(td, tdKey) in digits" :key="tdKey">{{ td }}</div>
  </div>
</template>

<script>
    export default {
        props: ['digits']        
    }
</script>

<style lang="scss">
  .playground {
    display: flex;
    flex-wrap: wrap;
    max-width: 250px;
  }
  .td {
    width: 62px;
    height: 60px;
    background: red;
    border: 2px solid white;
    border-radius: 10px;
    color: white;
    font-weight: bold;
    font-size: 2em;
  }
  .empty-td {
    background: gray;   
  }
</style>
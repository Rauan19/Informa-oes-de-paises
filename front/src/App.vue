<script setup>
 import Helloword from './components/Heme.vue'
 const numero1 = 1
</script>

<template>
  <div>
    <Helloword/>
    <p>{{ numero1 }}</p>
  </div> 
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

</style>

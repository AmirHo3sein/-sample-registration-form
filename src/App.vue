<script setup>
import {RouterView} from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <header class="flex justify-center items-center mt-24">
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125"/>
    <div class="wrapper">
      <HelloWorld msg="Registration Form"/>
    </div>
  </header>
  <RouterView/>
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}


header {
  display: flex;
  place-items: center;
}

.logo {
  margin: 0 2rem 0 0;
}

header .wrapper {
  display: flex;
  place-items: flex-start;
  flex-wrap: wrap;
}

nav {
  text-align: left;
  margin-left: -1rem;
  font-size: 1rem;
  padding: 1rem 0;
  margin-top: 1rem;
}

</style>

<script setup>
import Headline from './components/Headline.vue';
import FormatSelector from './components/FormatSelector.vue';
import { ref, onMounted  } from 'vue';

let quotes = ref();

onMounted( async function getQuotes() {
  const response = await fetch("https://api.oss117quotes.xyz/v1/characters");
  quotes.value = await response.json();
  quotes.value = quotes.value.flatMap(item => item.quotes);
})


</script>

<template>
  <header>
    <h1>Lorem ipOSS</h1>
  </header>
  <main>
    <div class="left">
      <Headline msg="GÉNÉRATEUR DE LOREM IPSUM OSS" />
      <h3>Parce qu’il faut faire cesser la dictature Lorem ipsum. <br>Non mais oh ! Comment tu parles de ton père ?! T'as pas honte ?"</h3>
    </div>
    <div class="right">
      <FormatSelector :quotes="quotes"/>
    </div>
  </main>
</template>

<style scoped>
</style>

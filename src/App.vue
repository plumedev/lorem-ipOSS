<script setup>
import Headline from './components/Headline.vue';
import Form from './components/Form.vue';
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
      <Headline msg="GÉNÉRATEUR DE LOREM IPSUM OSS 117" />
      <h3>
        Savez-vous seulement ce que c'est que Lorem ipsum ? Une dictature. Une dictature c'est quand les gens sont communistes, déjà. Qu'ils ont froid, avec des chapeaux gris et des chaussures à fermeture éclair. C'est ça, une dictature, Dolorès.
      </h3>
    </div>
    <div class="right">
      <Form :quotes="quotes"/>
    </div>
  </main>
  <div class="about">
    <h2>Pourquoi Lorem ipsum ?</h2>
    <p>Imagine que le Lorem Ipsum soit comme les répliques hilarantes et mémorables des films OSS 117. Ces répliques, tout comme le Lorem Ipsum, peuvent remplir l'espace et ajouter une touche d'humour et d'énergie, même si le contenu réel n'est pas encore là.</p>
    <p>Le Lorem Ipsum, c'est un peu comme les répliques cultes d'OSS 117 qui font rire et ajoutent du piquant aux dialogues. Ces phrases fictives, tout comme le Lorem Ipsum, permettent aux créateurs de donner un aperçu de l'apparence finale de leur projet, même avant d'y ajouter les éléments réels.</p>
    <p>Donc, le Lorem Ipsum, c'est un peu comme les punchlines mémorables d'OSS 117 dans le monde de la conception. Il ajoute ce petit quelque chose de spécial, tout en laissant place à l'imagination et à la créativité, tout comme nos aventures préférées d'OSS 117 au cinéma ! 😄</p>
  </div>
  <footer>Merci à <a href="https://oss117quotes.xyz/">oss117quotes.xyz</a> pour leur API.</footer>
</template>

<style scoped>
</style>

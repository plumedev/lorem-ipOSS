<script setup>
import { ref } from 'vue';

const props = defineProps({
  quotes: {
    type: Array,
    required: true
  }
})

const formats = ["sentences", "paragraphs"];

let qtyToGenerate = ref(1);
let selectedFormat = ref(formats[0]);
let textareaValue = ref("");
let selectedQuotes = ref([]);

function updateSelectedFormat(key) {
  selectedFormat.value = formats[key];
}

function generateIpsum() {
  if(selectedFormat.value === "sentences") {
    generateSentences();
  }
  if(selectedFormat.value === "paragraphs") {
    generateParagraphs();
  }
}

function generateSentences() {
  const shadowQuotes = [...props.quotes]
  selectedQuotes.value = [];
  for (let i = 0; i < qtyToGenerate.value; i++) {
    const randomIndex = Math.floor(Math.random() * props.quotes.length);
    selectedQuotes.value.push(shadowQuotes.splice(randomIndex, 1)[0]);
  }
}

function generateParagraphs() {
  const shadowQuotes = [...props.quotes];
  selectedQuotes.value = [];
  for (let i = 0; i < qtyToGenerate.value; i++) {
    const randomIndex = Math.floor(Math.random() * props.quotes.length);
    if(shadowQuotes[randomIndex].length >= 120) {
      selectedQuotes.value.push(shadowQuotes.splice(randomIndex, 1)[0]);
    } else {
      i--;
    }
  }

}
</script>

<template>
  <form @submit.prevent="generateIpsum">
    <fieldset>
      <legend>Je veux générer: {{ selectedFormat }}</legend>
  
      <input type="number" name="" id="" v-model="qtyToGenerate">

      <select v-model="selectedFormat">
        <option v-for="format, key in formats" :key="key">{{ format }}</option>
      </select>
  
      <button type="submit">Générer</button>
    </fieldset>
    <textarea id="story" name="story" rows="5" cols="33">
      {{ textareaValue }}
    </textarea>

    <div class="result">
      <p v-for="quote in selectedQuotes">
        {{ quote }}
      </p>

    </div>
  </form>
</template>

<style scoped>
</style>

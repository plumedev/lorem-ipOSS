<script setup>
import { ref } from 'vue';
import '@fortawesome/fontawesome-free/css/all.css';

const props = defineProps({
  quotes: {
    type: Array,
    required: true
  }
})

const formats = ["phrase(s)", "paragraphe(s)"];

let qtyToGenerate = ref(1);
let selectedFormat = ref(formats[0]);
let textareaValue = ref("");
let selectedQuotes = ref([]);
let copyLabel = ref("Copier");
let showErrorToManyParagraph = ref(false);

function updateSelectedFormat(key) {
  selectedFormat.value = formats[key];
}

function generateIpsum() {
  if(selectedFormat.value === "phrase(s)") {
    generateSentences();
  }
  if(selectedFormat.value === "paragraphe(s)") {
    generateParagraphs();
  }
}

function generateSentences() {
  const shadowQuotes = [...props.quotes]
  showErrorToManyParagraph = false;
  selectedQuotes.value = [];
  for (let i = 0; i < qtyToGenerate.value; i++) {
    const randomIndex = Math.floor(Math.random() * props.quotes.length);
    selectedQuotes.value.push(shadowQuotes.splice(randomIndex, 1)[0]);
  }
}

function generateParagraphs() {
  console.log('generateParagraphs')
  const shadowQuotes = [...props.quotes];
  selectedQuotes.value = [];
  showErrorToManyParagraph = false;
  if(qtyToGenerate.value < 7) {
    console.log('if')
    for (let i = 0; i < qtyToGenerate.value; i++) {
      const randomIndex = Math.floor(Math.random() * props.quotes.length - 1);
      if(shadowQuotes[randomIndex].length >= 120) {
        selectedQuotes.value.push(shadowQuotes.splice(randomIndex, 1)[0]);
      } else {
        i--;
      }
    }
  } else {
    console.log('else')
    showErrorToManyParagraph = !showErrorToManyParagraph;
  }
}

function copyQuotes() {
  const texts = document.querySelector('.texts');
  const textContent = texts.textContent || texts.innerText;
  
  showErrorToManyParagraph = false;
  navigator.clipboard.writeText(textContent)
    .then(() => {
      copyLabel.value = "Copié !";
    })
    .catch((err) => {
      console.error('Erreur lors de la copie dans le presse-papiers :', err);
    });
}
</script>

<template>
  <form @submit.prevent="generateIpsum">
    <fieldset>
      <legend>Je veux générer: </legend>
  
      <input name="qty" id="qty" v-model="qtyToGenerate">

      <select v-model="selectedFormat">
        <option v-for="(format, key) in formats" :key="key">{{ format }}</option>
      </select>
  
      <button type="submit">Générer</button>
    </fieldset>
  </form>
  <div id="result">
      <span class="error" v-if="showErrorToManyParagraph"><i class="fa-solid fa-triangle-exclamation"></i> Malheureusement OSS 117 n'est pas si loquace, essayer avec moins de répliques</span>
      <div class="texts">
        <p v-for="quote in selectedQuotes">
          {{ quote }}
        </p>
      </div>
      <button v-if="selectedQuotes != ''" class="copy" @click="copyQuotes">
        <i class="fa-solid fa-gun fa-flip-horizontal"></i>
        {{ copyLabel }}
      </button>
    </div>
</template>

<style scoped>
</style>

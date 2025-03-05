<script setup>
import { ref, computed } from "vue";

// Notas musicais
const notes = ["C", "C#", "D", "D#", "E", "F", "F#", "G", "G#", "A", "A#", "B"];

// Padrões das escalas
const majorScalePattern = [2, 2, 1, 2, 2, 2, 1]; // Maior (T-T-S-T-T-T-S)
const minorScalePattern = [2, 1, 2, 2, 1, 2, 2]; // Menor Natural (T-S-T-T-S-T-T)

// Mapeamento de transposição para Sax Alto (terça menor acima)
const transpositionMap = {
  "C": "A", "C#": "A#", "Db": "A#", "D": "B", "D#": "C", "Eb": "C", "E": "C#", 
  "F": "D", "F#": "D#", "Gb": "D#", "G": "E", "G#": "F", "Ab": "F", "A": "F#", 
  "A#": "G", "Bb": "G", "B": "G#"
};

// Tonalidade e tipo de escala selecionados pelo usuário
const inputKey = ref("C");
const scaleType = ref("major");

// Obtém o tom transposto
const transposedKey = computed(() => transpositionMap[inputKey.value] || "Desconhecido");

// Função para calcular escalas dinamicamente
function getScale(root, pattern) {
  let scale = [];
  let index = notes.indexOf(root);
  if (index === -1) return [];
  
  pattern.forEach(step => {
    scale.push(notes[index % notes.length]);
    index += step;
  });
  return scale;
}

// Obtém a escala transposta
const transposedScale = computed(() => {
  return scaleType.value === "major"
    ? getScale(transposedKey.value, majorScalePattern)
    : getScale(transposedKey.value, minorScalePattern);
});
</script>

<template>
  <div class="container">
    <h1>🎷 Transposição para Sax Alto</h1>
    
    <div class="input-group">
      <label for="tonalidade">Tom da Música:</label>
      <select id="tonalidade" v-model="inputKey">
        <option v-for="key in Object.keys(transpositionMap).sort()" :key="key" :value="key">
          {{ key }}
        </option>
      </select>
    </div>

    <div class="input-group">
      <label for="scaleType">Tipo de Escala:</label>
      <select id="scaleType" v-model="scaleType">
        <option value="major">Maior</option>
        <option value="minor">Menor</option>
      </select>
    </div>

    <div class="result">
      <h2>Sax Alto: <span>{{ transposedKey }}</span></h2>
      <h3>Escala {{ scaleType === "major" ? "Maior" : "Menor" }}: <span>{{ transposedScale.join(" - ") }}</span></h3>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 500px;
  margin: auto;
  text-align: center;
  font-family: Arial, sans-serif;
  padding: 20px;
  background: #d19a76;
  border-radius: 10px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

.input-group {
  margin-bottom: 15px;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

select {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #585252;
  border-radius: 5px;
}

.result {
  margin-top: 20px;
  background: #cccccc;
  color: #6b6b6b;
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.1);
}

h2 span, h3 span {
  color: #da9f5d;
  font-weight: bold;
}
</style>
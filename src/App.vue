<script setup>
import { ref, computed } from "vue";

// Mapeamento das transposições para Sax Alto (E♭)
const transpositionMap = {
  "C": "A", "C#": "A#", "Db": "A#", "D": "B", "D#": "C", "Eb": "C", "E": "C#", 
  "F": "D", "F#": "D#", "Gb": "D#", "G": "E", "G#": "F", "Ab": "F", "A": "F#", 
  "A#": "G", "Bb": "G", "B": "G#"
};

// Escalas maiores (Notação convencional)
const majorScales = {
  A: ["A", "B", "C#", "D", "E", "F#", "G#"],
  Bb: ["Bb", "C", "D", "Eb", "F", "G", "A"],
  B: ["B", "C#", "D#", "E", "F#", "G#", "A#"],
  C: ["C", "D", "E", "F", "G", "A", "B"],
  Db: ["Db", "Eb", "F", "Gb", "Ab", "Bb", "C"],
  D: ["D", "E", "F#", "G", "A", "B", "C#"],
  Eb: ["Eb", "F", "G", "Ab", "Bb", "C", "D"],
  E: ["E", "F#", "G#", "A", "B", "C#", "D#"],
  F: ["F", "G", "A", "Bb", "C", "D", "E"],
  Gb: ["Gb", "Ab", "Bb", "Cb", "Db", "Eb", "F"], // "Cb" representa B na armadura de clave
  G: ["G", "A", "B", "C", "D", "E", "F#"],
  Ab: ["Ab", "Bb", "C", "Db", "Eb", "F", "G"]
};

// Escalas menores (Notação convencional)
const minorScales = {
  A: ["A", "B", "C", "D", "E", "F", "G"],
  Bb: ["Bb", "C", "Db", "Eb", "F", "Gb", "Ab"],
  B: ["B", "C#", "D", "E", "F#", "G", "A"],
  C: ["C", "D", "Eb", "F", "G", "Ab", "Bb"],
  C#: ["C#", "D#", "E", "F#", "G#", "A", "B"],
  D: ["D", "E", "F", "G", "A", "Bb", "C"],
  Eb: ["Eb", "F", "Gb", "Ab", "Bb", "Cb", "Db"], // "Cb" representa B na armadura de clave
  E: ["E", "F#", "G", "A", "B", "C", "D"],
  F: ["F", "G", "Ab", "Bb", "C", "Db", "Eb"],
  F#: ["F#", "G#", "A", "B", "C#", "D", "E"],
  G: ["G", "A", "Bb", "C", "D", "Eb", "F"],
  Ab: ["Ab", "Bb", "Cb", "Db", "Eb", "Fb", "Gb"] // "Cb" = B e "Fb" = E
};


// Estado da tonalidade e tipo de escala
const inputKey = ref("C");
const scaleType = ref("major");

// Cálculo do tom transposto
const transposedKey = computed(() => transpositionMap[inputKey.value] || "Desconhecido");

// Obtém a escala correta (maior ou menor)
const transposedScale = computed(() => {
  return scaleType.value === "major" ? majorScales[transposedKey.value] : minorScales[transposedKey.value];
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
      <label for="escala">Tipo de Escala:</label>
      <select id="escala" v-model="scaleType">
        <option value="major">Maior</option>
        <option value="minor">Menor</option>
      </select>
    </div>

    <div class="result">
      <h2>Sax Alto: <span>{{ transposedKey }}</span></h2>
      <h3>Escala: <span>{{ transposedScale.join(" - ") }}</span></h3>
    </div>
  </div>
  <div class="majors">
    <h1>Escalas maiores Sustenidas</h1>
    <p>Do</p>
    <p>Sol ---- Sol La Si Do Re Mi Fa#</p>
    <p>Re ----- Re Mi Fa# Sol La Si Do#</p>
    <p>La ----- La Si Do# Re Mi Fa# Sol#</p>
    <p>Mi ----- Mi Fa# Sol# La Si Do# Re#</p>
    <p>Si ----- Si Do# Re# Mi Fa# Sol# La#</p>
    <p>Fa# ---- Fa# Sol# La# Si Do# Re# Mi#</p>
    <p>Do# ---- Do# Re# Mi# Fa# Sol# La# Si#</p>
    <p>Fa ----- Fa Sol La Sib Do Re Mi</p>
    <p>Sib ---- Sib Do Re Mib Fa Sol La</p>
    <p>Mib ---- Mib Fa Sol Lab Sib Do Re Mib</p>
    <p>Lab ---- Lab Sib Do Reb Mib Fa Sol Lab</p>
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

.majors {
  max-width: 500px;
  margin: auto;
  text-align: left;
  font-family: Arial, sans-serif;
  padding: 20px;
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

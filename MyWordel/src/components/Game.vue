<script setup>

import { ref, watch, computed} from 'vue';

import wordsData from '../assets/words.json'; // Passe den Pfad entsprechend an
const emit = defineEmits(['won', 'lost']);

const currentInput = ref('');
const inputs = ref([]);
const outputs = ref([]);
//const additionalOutput = ref('');
const gameWon = ref(false);
const word = ref('');


function selectRandomWord() {
  const words = wordsData.words;
  if (!words || words.length === 0) {
  throw new Error('Das Wörter-Array ist leer oder nicht definiert.');
  } 

  const randomIndex = Math.floor(Math.random() * words.length);
  word.value = words[randomIndex];

  console.log("(Randomwort) word.value: " +word.value); //TODO: entfernen wenn fertig
  
}

selectRandomWord(); //random wort wählen

function processInput(){
  //if(currentInput.value != ''){
    //console.log("wir sind in processInput mit input: " + currentInput.value )
  //}


  
  if (inputs.value.length < 5 && !gameWon.value){
    inputs.value.push(currentInput.value);
    const result = startWordelGame(currentInput.value);
    outputs.value.push(result);
    currentInput.value = ''; 
  }

}

function resetGame() {
  currentInput.value = '';
  inputs.value = [];
  outputs.value = [];
  gameWon.value = false;
  selectRandomWord(); //neues zufälliges wort wählen
}


function startWordelGame(input) {
  let result = '';
  if (word.value == input) { // Wenn das eingegebene Wort dem gesuchten Wort entspricht, gewinnen wir
    gameWon.value = true;
    result = `<span style="color:green;">${input}</span>`;
  } else {
    for (let i = 0; i < 5; i++) { // Wir durchlaufen das ganze Wort
      if (word.value[i] == input[i]) {
        result += `<span style="color:green;">${input[i]}</span>`; // Wenn der Buchstabe korrekt ist, färben wir ihn grün
      } else if (word.value.includes(input[i])) {
        result += `<span style="color:orange;">${input[i]}</span>`; // Wenn der Buchstabe im gesuchten Wort enthalten ist, färben wir ihn orange
      } else {
        result += input[i]; //Andernfalls bleibt der Buchstabe unverändert
      }
    }
  }
  return result;
}


watch(gameWon, (newValue, oldValue) => { //wir watchen die gamewon variable, wenn sich was verändernt schicken wir nach oben zu app 
  if (newValue === true) {
    // When gameWon becomes true, emit the event
      emit('won'); //an App.vue übergeben wir, dass wir gewonnen haben
    
  }
});

const gameStatus = computed(() => { //für anzahl wie oft wir verloren haben
  return {
    inputLength: inputs.value.length,
    gameWon: gameWon.value
  };
});

// Watcher für emit lost
watch(gameStatus, (newStatus) => {
  if (newStatus.inputLength >= 5 && !newStatus.gameWon) {
    //console.log("inputs.length ist größer gleich 5 und gameWon ist false");
    emit('lost'); //schicken an app das wir verloren haben
  }
});


</script>

<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-6">
        <div id="spiel">
          <form @submit.prevent="processInput" v-if="inputs.length < 5 && !gameWon">
            <div id="eingabe" class="input-wrapper mt-4">
              <input type="text" v-model="currentInput" minlength="5" maxlength="5" id="input" name="text" placeholder="Type here..." class="input form-control" autocomplete="off">
            </div>
            <div class="mt-4">
              <button type="submit" :disabled="currentInput.length === 0" class="btn btn-primary">Absenden</button>
            </div>
          </form>

          <div v-if="gameWon" id="gewonnen" class="mt-4 containerGif">
            <h3>Gewonnen!</h3>
            <img src="../assets/smirkCat.gif" alt="Gewonnen, Smirking Cat Gif">
          </div>

          <div v-if="inputs.length >= 5 && !gameWon" id="verloren" class="mt-4 containerGif">
            <h3 >Verloren! Das gesuchte Wort war: {{ word }}</h3>
            <img src="../assets/gameOver.gif" alt="Game Over, Katze fällt zu Boden Gif">
          </div>

          <div class="mt-4">
            <button type="button" class="buttonRefresh" @click="resetGame">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-arrow-repeat" viewBox="0 0 16 16">
                <path d="M11.534 7h3.932a.25.25 0 0 1 .192.41l-1.966 2.36a.25.25 0 0 1-.384 0l-1.966-2.36a.25.25 0 0 1 .192-.41zm-11 2h3.932a.25.25 0 0 0 .192-.41L2.692 6.23a.25.25 0 0 0-.384 0L.342 8.59A.25.25 0 0 0 .534 9z"></path>
                <path fill-rule="evenodd" d="M8 3c-1.552 0-2.94.707-3.857 1.818a.5.5 0 1 1-.771-.636A6.002 6.002 0 0 1 13.917 7H12.9A5.002 5.002 0 0 0 8 3zM3.1 9a5.002 5.002 0 0 0 8.757 2.182.5.5 0 1 1 .771.636A6.002 6.002 0 0 1 2.083 9H3.1z"></path>
              </svg>
              Reset
            </button>
          </div>


        </div>
      </div>

      <div class="col-md-6">
        <div v-if="outputs.length > 0" id="ausgaben" class="output-wrapper">
            <h3>Ausgaben:</h3>
            <div style="background-color: lightgrey;">
              <ul>
                <li style="font-weight: 500; font-size: xx-large;" v-html="output" v-for="(output, index) in outputs" :key="index"></li>
              </ul>
            </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

#spiel{
  margin: 20px;
  padding: 20px;
  border: 1px solid black;
  border-radius: 5px;
  background-color: white;
}

.containerGif img {
  width: 100%;
  height: 100%;
  object-fit: contain; /* Bild skalieren und Seitenverhältnis beibehalten */
}

.output-wrapper {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}


.input-wrapper input { 
  background-color: #eee;
  border: none;
  padding: 1rem;
  font-size: 1rem;
  width: 13em;
  border-radius: 1rem;
  color: rgb(0, 0, 0);
  box-shadow: 0 0.4rem #dfd9d9;
  cursor: pointer;
}

.input-wrapper input:focus { 
  outline-color: rgb(0, 0, 0);
}

/*
Copyright - 2024 adamgiebl (Adam Giebl) 

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
*/


.buttonRefresh {
  color: white;
  background-color: #222;
  font-weight: 500;
  border-radius: 0.5rem;
  font-size: 1rem;
  line-height: 2rem;
  padding-left: 2rem;
  padding-right: 2rem;
  padding-top: 0.7rem;
  padding-bottom: 0.7rem;
  cursor: pointer;
  text-align: center;
  margin-right: 0.5rem;
  display: inline-flex;
  align-items: center;
  border: none;
}

.buttonRefresh:hover {
  background-color: #333;
}

.buttonRefresh svg {
  display: inline;
  width: 1.3rem;
  height: 1.3rem;
  margin-right: 0.75rem;
  color: white;
}

.buttonRefresh:focus svg {
  animation: spin_357 0.5s linear;
}

@keyframes spin_357 {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}

/*
Copyright - 2024 JaydipPrajapati1910 (Jaydip Prajapati) 

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
*/



</style>

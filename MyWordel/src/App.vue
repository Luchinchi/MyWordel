<script setup>
import { ref } from 'vue';
import Game from './components/Game.vue'
import Header from './components/Header.vue'
import Modal from '@/components/Modal.vue'


const title = 'MyWordel'
let won = ref(0); //wie oft haben wir gewonnen
let showCelebration = ref(false); // track celebration state

let lost = ref(0); //wie oft haben wir verloren

let streak = ref(0); //streaks

//let lost = ref(0); //wie oft wir verloren haben

function incrementWon(){ //gewinnen hochzählen
  won.value++;
  streak.value++; //streak hochzählen
  console.log("wir haben " + won.value + " mal gewonnen"); //TODO entfernen wenn fertig

  if (won.value === 50) { // trigger celebration on 50 wins //easter egg
    showCelebration.value = true;
    setTimeout(() => {
      showCelebration.value = false;
    }, 3000); // celebration duration
  }
}

function incrementLost(){ //verlieren hochzählen
  lost.value++;
  streak.value=0;
  console.log("Wir haben " + lost.value + " mal verloren") 
}

</script>

<template>

  <Header :title="title" :won="won" :streak="streak"> </Header>
  <Game @won="incrementWon()" @lost="incrementLost()"></Game>
  <Modal :won="won" :lost="lost"></Modal>

  <div v-if="showCelebration" class="celebration">
    <div class="sparkles">✨✨✨</div>
    <div class="confetti">🎉🎉🎉</div>
  </div>
  
</template>

<style scoped>

.celebration {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  animation: pulse 1s infinite;
}

.sparkles {
  font-size: 3rem;
  color: gold;
  animation: sparkle 1s infinite;
}

.confetti {
  font-size: 3rem;
  color: deepskyblue;
  animation: confetti 1s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); opacity: 1; }
  50% { transform: scale(1.1); opacity: 0.7; }
  100% { transform: scale(1); opacity: 1; }
}

@keyframes sparkle {
  0%, 100% { opacity: 1; transform: translateY(0); }
  50% { opacity: 0.5; transform: translateY(-10px); }
}

@keyframes confetti {
  0%, 100% { opacity: 1; transform: translateY(0); }
  50% { opacity: 0.5; transform: translateY(10px); }
}

</style>

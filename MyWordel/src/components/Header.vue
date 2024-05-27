<script setup>
import { ref, watch } from 'vue';
import 'bootstrap';

const props = defineProps(['title', 'won', 'streak']);
const badgeRef = ref(null);

watch(() => props.won, () => { //jedesmal wenn won sich verändert entfernen wir die klasse und fügen die hinzu, sodass animation nochmal
  if (badgeRef.value && props.won<10) {
    badgeRef.value.classList.remove('animiere');
    // Reflow trick: Reinsert the element or force a reflow to restart the animation
    void badgeRef.value.offsetWidth;
    badgeRef.value.classList.add('animiere');

  } else if (badgeRef.value && props.won>=10 && props.won <20){ //zwischen 5 bis 9 siege
    badgeRef.value.classList.remove('animiere2');
    // Reflow trick: Reinsert the element or force a reflow to restart the animation
    void badgeRef.value.offsetWidth;
    badgeRef.value.classList.add('animiere2');

  }else if (badgeRef.value && props.won>=20) { //ab 10 siege
    badgeRef.value.classList.remove('animiere3');
    // Reflow trick: Reinsert the element or force a reflow to restart the animation
    void badgeRef.value.offsetWidth;
    badgeRef.value.classList.add('animiere3');
  }

});
</script>

<template>
  <nav class="navbar bg-light">
    <div class="container-fluid">
      <!-- Logo and title-->
      <span class="navbar-brand mb-0 h1">
        <img src="../../favicon2.ico" alt="Logo" width="24" height="24" class="d-inline-block align-text-top"> 
        {{ title }}
      </span>
      <span class="badge ms-2 wonn" style="background-color: slategray;" ref="badgeRef">
        🏆 {{ won }}
      </span>
      <span class="badge ms-2 wonn" style="background-color: slategray;">
        🔥 Streak: {{streak}}  
      </span>
      <button id="buttonToggleModal" class="btn btn-outline-primary" data-bs-toggle="modal" data-bs-target="#modalRoot">
        Spielregeln&Stats
      </button>
    </div>
  </nav>
</template>

<style>
.navbar {
  border: 1px solid #ccc;
  border-radius: 5px;
}

@keyframes pulse {
  0% {
    background-color: slategray;
    transform: scale(1);
  }
  25% {
    background-color: #6a5acd; /* SlateBlue */
    transform: scale(1.1);
  }
  50% {
    background-color: #7fffd4; /* Aquamarine */
    transform: scale(1.2);
  }
  75% {
    background-color: #ffa07a; /* LightSalmon */
    transform: scale(1.1);
  }
  100% {
    background-color: slategray;
    transform: scale(1);
  }
}

.animiere {
  animation: pulse 2s ease-in-out;
  animation-duration: 4s;
  display: inline-block;
  padding: 0.2em 0.5em;
  border-radius: 0.25rem;
}




@keyframes crazyPulse {
  0% {
    background-color: slategray;
    transform: scale(1) rotate(0deg);
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
  }
  20% {
    background-color: #ff69b4; /* HotPink */
    transform: scale(1.2) rotate(10deg);
    box-shadow: 0 0 15px rgba(255, 105, 180, 0.7);
  }
  40% {
    background-color: #00ffff; /* Aqua */
    transform: scale(1.4) rotate(-10deg);
    box-shadow: 0 0 25px rgba(0, 255, 255, 0.7);
  }
  60% {
    background-color: #7fff00; /* Chartreuse */
    transform: scale(1.6) rotate(10deg);
    box-shadow: 0 0 35px rgba(127, 255, 0, 0.7);
  }
  80% {
    background-color: #ff4500; /* OrangeRed */
    transform: scale(1.8) rotate(-10deg);
    box-shadow: 0 0 45px rgba(255, 69, 0, 0.7);
  }
  100% {
    background-color: slategray;
    transform: scale(1) rotate(0deg);
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
  }
}

.animiere2 {
  animation: crazyPulse 4s ease-in-out;
  animation-duration: 4s;
  display: inline-block;
  padding: 0.2em 0.5em;
  border-radius: 0.25rem;
}



@keyframes crazyFlying {
  0%, 100% {
    transform: translateX(0) rotate(0deg) scale(1);
    background-color: #ff6347;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  }
  20% {
    transform: translateX(20vw) rotate(0deg) scale(1.2);
  }
  40% {
    transform: translateX(-20vw) rotate(0deg) scale(1.4);
  }
  60% {
    transform: translateX(0) rotate(180deg) scale(1.6);
    background-color: #ffd700;
  }
  80% {
    transform: translateX(0) rotate(360deg) scale(2);
    background-color: #ff69b4;
  }
}

.animiere3 {
  background-color: tomato;
  animation: crazyFlying 4s ease-in-out;
  animation-duration: 4s;
  display: inline-block;
  padding: 0.2em 0.5em;
  border-radius: 0.25rem;
  
}

.wonn{
  color: white;
  font-size: 1.5rem;
  transform-origin: center center;

}

</style>
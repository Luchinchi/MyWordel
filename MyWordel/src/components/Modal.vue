<script setup>
import { computed } from 'vue';

// Props definieren
const props = defineProps(['won', 'lost']);

// Funktion zur Berechnung der durchschnittlichen Gewinnrate
function calculateAverageWinRate(won, lost) {
  const totalGames = won + lost;
  if (totalGames === 0) {
    return 0; // Vermeidung der Division durch null
  }
  return (won / totalGames) * 100; // Gewinnrate in Prozent
}

// Aufruf der Funktion, um die durchschnittliche Gewinnrate zu berechnen
const averageWinRate = computed(() => calculateAverageWinRate(props.won, props.lost)); //computed => berechnung nur neu, wenn sich won oder lost verändert 
</script>

<template>
    <div id="modalRoot" class="modal fade" tabindex="-1" aria-labelledby="modalRootLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header d-inline-flex -align items-center">
                    <h5 class="modal-title me-3">Spielregeln</h5>
                    <button id="modalButtonX" type="button" class="btn btn-close" data-bs-dismiss="modal"></button>
                </div>
                <div class="modal-body">
                    <div class="container mt-4">
                        <div class="rules">
                            <ul>
                                <li>Du hast 5 versuche um das Wort der länge 5 zu erraten.</li>
                                <li>Umlaute sind zweistellig z.B. ü wird zu ue.</li>
                                <li>Wenn es der richtige <b>Buchstabe</b> ist und die <b>Position</b> auch richtig ist, dann ist der Buchstabe <span class="outlined" style="color:green;">Grün</span>.</li>
                                <li>Wenn der Buchstabe im gesuchten wort enthalten ist, dann ist er <span class="outlined" style="color:orange; ">Orange</span>. <p>Beachte: Es könnten sich zwei gleiche Buchstaben Orange färben, im gesuchten Wort könnte sich jedoch nur eine davon befinden.</p></li>
                            </ul>
                        </div>
                        
                        <div class="mt-4 stats">
                            <p>Anzahl gewonnener Spiele: {{ won }}</p>
                            <p>Anzahl verlorener Spiele: {{ lost }}</p>
                            <p>Durchschnittliche Gewinnrate {{ averageWinRate.toFixed(2) }}%</p> <!-- auf zwei nachkommastellen runden -->

                        </div>


                    </div>
                    
                </div>

            </div>

        </div>

    </div>


</template>

<style>

.rules {
  margin: 20px;
  padding: 20px;
  border: 1px solid black;
  border-radius: 5px;
  background-color: #cec9c9;
}

.outlined{
  text-shadow:
        -1px -1px 0 #000,  
        1px -1px 0 #000,
        -1px  1px 0 #000,
        1px  1px 0 #000; /* Schwarz als Outline-Farbe */
}

.stats {
  margin: 20px;
  padding: 20px;
  border: 1px solid #9e0d0d; 
  border-radius: 10px;
  background-color: #f0f0f0; /* Hellgrauer Hintergrund */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Leichter Schatten */
}

.stats p {
  margin: 5px 0;
  font-size: 16px; /* Schriftgröße */
  color: #333; /* Dunkelgraue Schrift */
}

</style>
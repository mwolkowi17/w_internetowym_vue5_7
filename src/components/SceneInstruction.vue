<script setup>
import { onMounted } from "vue";
import { instrukcjaTekst } from "../lib/instrukcja";
defineEmits(["koniec-instrukcja", "koniec-instrukcja-focus"]);

const props = defineProps({
  ifButtonOnFocus: Boolean,
});

onMounted(() => {
  const elementToFocus = document.querySelector(".info-instrukcja");
  //dodanie warunku propsu
  if (elementToFocus && props.ifButtonOnFocus === true) {
    elementToFocus.focus();
  }
});

const textToDisplay =
  "Gra składa się z dwóch poziomów. Na początku otrzymujesz trzy szanse." +
  " Rzuć kostką i idź do przodu o tyle pól, ile oczek wypadło na kostce.";

const pytajnikImage = new URL("../assets/ikona2question.png", import.meta.url)
  .href;
const textToDisplay2 =
  "<img class='pytajnik' alt='' src=" +
  pytajnikImage +
  ">– na polach ze znakiem zapytania czeka cię pytanie.";

const wykrzyknikImage = new URL(
  "../assets/ikona3wykrzyknik.png",
  import.meta.url
);

const textToDisplay3 =
  "<img class='wykrzyknik' alt='' src=" +
  wykrzyknikImage +
  ">– na polach z wykrzyknikiem czeka cię niespodzianka.";

const textToDisplay4 =
  "Poprawna odpowiedź – rzucasz kostką ponownie. Niepoprawna odpowiedź" +
  " – tracisz jedną szansę. Jeśli wygrasz poziom 1, to otrzymujesz dodatkową szansę." +
  " Utrata wszystkich szans oznacza koniec gry. Powodzenia!";

const textToDisplayCzytnik =
  "Zasady gry " +
  "Gra składa się z dwóch poziomów. Na początku otrzymujesz trzy szanse." +
  " Rzuć kostką i idź do przodu o tyle pól, ile oczek wypadło na kostce." +
  "– na polach ze znakiem zapytania czeka cię pytanie." +
  "– na polach z wykrzyknikiem czeka cię niespodzianka." +
  "Poprawna odpowiedź – rzucasz kostką ponownie. Niepoprawna odpowiedź" +
  "– tracisz jedną szansę. Jeśli wygrasz poziom 1, to otrzymujesz dodatkową szansę." +
  "Utrata wszystkich szans oznacza koniec gry. Powodzenia!" +
  " Po grze możesz poruszać się za pomocą klawiszy TAB, kombinacji klawiszy Shift i TAB oraz klawisza Enter, którym zatwierdzasz swój wybór" +
  " W grze usłyszysz także sygnały dźwiękowe, oznaczające odpowiednio ruch pionka, prawidłową lub złą odpwiedź, wygraną etapu lub wygraną całej gry oraz dźwięk" +
  "informujący cię o przegranej";

//dodać informacje dla osób niewidzących
</script>
<template>
  <div class="tlo1" aria-label="Zasady gry">
    <div
      class="info-instrukcja"
      tabindex="0"
      :aria-label="textToDisplayCzytnik"
    >
      <h1 class="instrukcja-title">Zasady gry</h1>
      <div class="kontener-instrukcja">
        <p class="instrukcja" v-html="textToDisplay"></p>
        <!-- <img class="gwiazdka" alt="" src="../assets/ikona1gwiazdka.png" />-->
        <!-- <img class="pytajnik" alt="" src="../assets/ikona2question.png" /> -->
        <p class="instrukcja" v-html="textToDisplay2"></p>

        <p class="instrukcja" v-html="textToDisplay3"></p>
        <!-- <img class="wykrzyknik" alt="" src="../assets/ikona3wykrzyknik.png" /> -->
        <p class="instrukcja" v-html="textToDisplay4"></p>
      </div>
    </div>
  </div>
  <button
    class="dalej anim1"
    @click="$emit('koniec-instrukcja')"
    @keydown.enter="$emit('koniec-instrukcja-focus')"
    role="button"
  >
    Dalej
  </button>
</template>

<style scoped>
.tlo1 {
  background-image: url("../assets/plansza_zasady_gry.png");
  background-size: 1920px 1080px;
  height: 1080px;
  width: 1920px;
  top: 0px;
  left: 0px;
  position: absolute;
}

.info-instrukcja {
  height: 680px;
  width: 1520px;
  top: 120px;
  left: 197px;
  position: absolute;
}

.info-instrukcja:focus {
  outline: 2px solid #ffffff;
}

.instrukcja-title {
  position: absolute;
  color: rgb(255, 255, 255);
  font-size: 110px;
  font-style: normal;
  font-weight: 400;
  font-family: "Proxima Nova", sans-serif;
  line-height: 1.5;
  top: -115px;
  left: 475px;
  text-align: center;
}

.kontener-instrukcja {
  /* display: flex;
  align-items: center; */
  display: inline-block;
  padding: 0.5rem 1rem;
  /* width: 1300px; */
  width: 1400px;
  top: 90px;
  left: 55px;
  position: absolute;
}

.instrukcja {
  color: rgb(255, 255, 255);
  font-size: 41px;
  font-style: normal;
  font-weight: 300;
  font-family: "Proxima Nova", sans-serif;
  line-height: 1.7;
  position: relative;
  margin-bottom: -20px;
}

.gwiazdka {
  position: relative;
  /* top: 220px;
  left: 746px; */
  margin-top: -45px;
  margin-right: -95px;
  left: -98px;
  top: 35px;
}

.pytajnik {
  /* position: absolute; */
  top: 283px;
  left: 1355px;
  /* margin-top: -38px;
  margin-right: -80px;
  left: -90px; */
  /*top: 35px;*/
}

.kontener-instrukcja :deep(.pytajnik) {
  position: relative;

  margin-top: -308px;
  margin-right: -10px;
  left: -10px;
  top: 35px;
}

.wykrzyknik {
  position: relative;
  /* top: 475px;
  left: 715px; */
  /* margin-top: -38px;
  margin-right: -80px;
  left: -90px; */
  /* top: 35px;*/
}

.kontener-instrukcja :deep(.wykrzyknik) {
  position: relative;

  margin-top: -308px;
  margin-right: -10px;
  left: -10px;
  top: 35px;
}
.dalej {
  color: rgb(29, 56, 80);
  font-size: 70px;
  font-style: bold;
  font-weight: 700;
  font-family: "Proxima Nova", sans-serif;
  position: absolute;
  top: 820px;
  left: 850px;
  width: 301px;
  height: 117px;
  border: 4px solid rgb(0, 187, 255);
  /* display: flex;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
  overflow: hidden;           /* Prevent overflow *
  transform-origin: center;  */
  transition: 0.2s ease-in-out;
}

.dalej:hover {
  cursor: pointer;
  transform: scale(1.1);
}

.dalej:focus {
  outline: 5px solid white;
  outline-offset: 10px;
}

/* The animation code */
@keyframes example {
  from {
    opacity: 0;
  }

  to {
    opacity: 100;
  }
}

/* The element to apply the animation to */
.anim1 {
  animation-name: example;
  animation-duration: 1s;
}
</style>

<script setup>
import { ref, onMounted, useTemplateRef, nextTick } from "vue";
import { metodyPomocnicze } from "../lib/metody-pomocnicze";
import { PawnMaps } from "../lib/pawn-maps";
import { Traps } from "../lib/traps";
import SceneQuizz1 from "./SceneQuizz1.vue";
import SceneTrap from "./SceneTrap.vue";

const emit = defineEmits([
  "koniec-etap1",
  "przegrana",
  "koniec-etap1-focus",
  "przegrana-focus",
  "odejmij-gwiazdke",
  "reset-gwiazdek",
]);

const props = defineProps({
  ifButtonOnFocusMain1: Boolean,
});

defineOptions({
  inheritAttrs: false,
});
//obsługa focusa
const ifQuizzFocusOn = ref(false);
const ifTrapFocusOn = ref(false);
const ifRzucKostkaButtonOnFocus = ref(false);
const ifFocusEmitGlobal = ref(false);

//referencje do el html używane do obsługi focusa
const button_rzut = useTemplateRef("rzut1");
const napisRuch = useTemplateRef("ruchGracza");
const kostkaOczka = useTemplateRef("oczkaKostka");

onMounted(() => {
  if (props.ifButtonOnFocusMain1 === true) {
    button_rzut.value.focus();
  }

  emit("reset-gwiazdek");
});

//roboczo tylko dla starej funkcji
const postac1 = ref("postać");

//pozycja startowa gracza nr 1
const krok_gracz1_na_planszy = ref(0);

//roboczo do edycji pytań
//const krok_gracz1_na_planszy = ref(15);

//zdefinowanie pozycji (mapy wszystkich pozycji) gracza nr 1
const pozycje_pionka_gracza1 = new PawnMaps().pionek_gracza1;

//początkowa ilość "szans"
const ilosc_szans = ref(3);

//widoczność przycisku Rzuć kostką
const if_rzuc_kostka = ref(true);

//informacja o ruchu gracza
const if_ruch_gracza = ref(false);

//widoczność kostki
const if_widok_kostki = ref(false);

//widoczność planszy pułapka
const if_widok_pulapki = ref(false);

//widoczność planszy quizz1
const if_widok_quizz1 = ref(false);
//roboczo do edycji pytań
//const if_widok_quizz1 = ref(true);

//wartość wskazujaca rodzaj pulapki rodzaju pułapki
const trapType = ref(0);

//wartości propsów planszy zasadzka
const titleTrap = ref("Zasadzka!");
const textTrap = ref("Cofasz się o 2 pola.");

//widoki szans na planszy
const if_szansa1 = ref(true);
const if_szansa2 = ref(true);
const if_szansa3 = ref(true);

let kolekcja_widoków_kostki = [false, false, false, false, false, false];

const isSet1 = ref(kolekcja_widoków_kostki[0]);
const isSet2 = ref(kolekcja_widoków_kostki[1]);
const isSet3 = ref(kolekcja_widoków_kostki[2]);
const isSet4 = ref(kolekcja_widoków_kostki[3]);
const isSet5 = ref(kolekcja_widoków_kostki[4]);
const isSet6 = ref(kolekcja_widoków_kostki[5]);

//pozycja pionka
const pionek_left = ref(30);
const pionek_top = ref(330);

//roboczo do ustaewienia pozycji pionka
// const pionek_left = ref(pozycje_pionka_gracza1[15][0])
// const pionek_top = ref(pozycje_pionka_gracza1[15][1])

//flaga true/false pokazująca czy gracz nr 1 nie przeszedł całej planszy, wartość falsce wskazuje zakończenie ruchu na planszy
let kontrolka_ruch_na_planszy = true;

// licznik ruchu na planszy - faktyczny ruch pionka
let ruch_lokalny = 0;

// zmienna robocza używana przy liczbie wyrzuconych oczek
let x;

//instancja obieku odpowiadającego za pułapki
const trap = new Traps();

// nowa funkcjonalnosc ograniczająca ilośc wpadek - zmienne sterujace - trzeba dodać dodawanie wartosci-liczba wpadek-przy pułapce!!!!
const liczba_wyrzucona = ref(0);
const liczba_wpadek = ref(0);

const wyrzuconaWartoscKostki = ref("Kostka - liczba oczek: " + (x + 1));

let sound_ruch = new Audio(
  new URL("../assets/ruch_pionka.mp3", import.meta.url).href
);
sound_ruch.preload = "auto";

async function kostka_click() {
  if_ruch_gracza.value = true;

  await nextTick();

  if_rzuc_kostka.value = false; //  ukryj przycisk rzuć kostką

  //========================================================================================
  let i = 0; //  set your counter to 0
  //========================================================================================
  if_widok_kostki.value = true;
  console.log("rzut");

  //funkcjonalnosc ograniczająca ilośc wpadek
  let wartoscWyrzuconaFirst = metodyPomocnicze.rzucaj();
  console.log("oczka: " + wartoscWyrzuconaFirst);
  await nextTick();
  kostkaOczka.value.focus();
  if (liczba_wpadek.value < 2) {
    console.log("ilość wpadek: " + liczba_wpadek.value);
    liczba_wyrzucona.value = wartoscWyrzuconaFirst;
  }
  if (
    liczba_wpadek.value >= 2 &&
    trap.czy_polapka(
      krok_gracz1_na_planszy.value + wartoscWyrzuconaFirst + 1
    ) === true
  ) {
    console.log("zmieniam");

    if (wartoscWyrzuconaFirst < 5) {
      liczba_wyrzucona.value = wartoscWyrzuconaFirst + 1;
    } else {
      liczba_wyrzucona.value = wartoscWyrzuconaFirst - 1;
    }
  } else {
    console.log("ilość wpadek powyżej: " + liczba_wpadek.value);
    liczba_wyrzucona.value = wartoscWyrzuconaFirst;
  }
  //========================================koniec funcjonalnosci ograniczającej liczbę wpadek===============================================

  x = liczba_wyrzucona.value;
  wyrzuconaWartoscKostki.value = "Kostka - liczba oczek: " + (x + 1);
  let wynik_rzutu = x;
  console.log(x);
  for (let i = 0; i < 6; i++) {
    if (i === x) {
      kolekcja_widoków_kostki[i] = true;
    } else {
      kolekcja_widoków_kostki[i] = false;
    }

    isSet1.value = kolekcja_widoków_kostki[0];
    isSet2.value = kolekcja_widoków_kostki[1];
    isSet3.value = kolekcja_widoków_kostki[2];
    isSet4.value = kolekcja_widoków_kostki[3];
    isSet5.value = kolekcja_widoków_kostki[4];
    isSet6.value = kolekcja_widoków_kostki[5];
  }

  console.log(kolekcja_widoków_kostki);

  //!!============================ruch pionka loop =========================================
  const myLoopPionek = (arg_A, arg_B, arg_C) => {
    //  loop function
    setTimeout(async function () {
      //call a 1s setTimeout when the loop is called
      //efekt dźwiękowy ruchu pionka
      sound_ruch = new Audio(
        new URL("../assets/ruch_pionka.mp3", import.meta.url).href
      );
      await nextTick();
      sound_ruch.play();
      await nextTick();
      pionek_left.value = arg_B[arg_C.value + i][0];
      pionek_top.value = arg_B[arg_C.value + i][1];

      //robocze ustawienie pozycji pionka na planszy dla celów ustwień bibliotek
      // pionek_left.value = arg_B[1][0]
      // pionek_top.value = arg_B[1][1]

      console.log(arg_C.value);
      console.log(arg_B[arg_C.value + i]);

      if (ruch_lokalny >= 15) {
        console.log("Zwycięstwo!");
        kontrolka_ruch_na_planszy = false;
        console.log("wygrałeś!!!");
        wywolanie_sceny_koncowej();
      }

      ruch_lokalny++;

      i++; //  increment the counter

      if (i <= wynik_rzutu && ruch_lokalny <= 15) {
        myLoopPionek(arg_A, arg_B, arg_C); //  ..  again which will trigger another
      } else {
        dodanie_krokow();
        pulapka_czy_quizz();
      }
    }, 1000);
  };

  function dodanie_krokow() {
    krok_gracz1_na_planszy.value =
      krok_gracz1_na_planszy.value + wynik_rzutu + 1;
  }

  if (kontrolka_ruch_na_planszy === true) {
    //  start the loop
    myLoopPionek(postac1, pozycje_pionka_gracza1, krok_gracz1_na_planszy);

    console.log("krok na planszy: " + krok_gracz1_na_planszy.value);
  }

  const pulapka_czy_quizz = async () => {
    console.log("sprawdzam czy pułapka albo quizz");
    console.log(i);
    console.log("wynik rzutu: " + wynik_rzutu);
    console.log("kontrolka ruch na planszy: " + kontrolka_ruch_na_planszy);
    if (kontrolka_ruch_na_planszy === true) {
      console.log("pulapka albo quizz!!!");
      console.log("krok gracza na planszy: " + krok_gracz1_na_planszy.value);
      // sprawdzenie czy gracz wpadł w pułapkę
      console.log(trap.czy_polapka(krok_gracz1_na_planszy.value));
      //  tu w momencie kiedy gracz zanjdzie się na polu pułapka będzie go cofało a jak nie to odpala quizz
      if (trap.czy_polapka(krok_gracz1_na_planszy.value) === true) {
        console.log("wpadka");
        //dodaje wpadki do licznika wpadek
        liczba_wpadek.value = liczba_wpadek.value + 1;
        //  pokazuje planszę pułapki
        setTimeout(async () => {
          if_widok_pulapki.value = true;
          await nextTick();
          titleTrap.value = metodyPomocnicze.pokazTekstPulapki(
            krok_gracz1_na_planszy.value
          )[0];
          textTrap.value = metodyPomocnicze.pokazTekstPulapki(
            krok_gracz1_na_planszy.value
          )[1];
          const sound_cofasz = new Audio(
            new URL("../assets/zla_odp.mp3", import.meta.url).href
          );
          sound_cofasz.play();
        }, 1000);
      } else {
        console.log("quiz");
        setTimeout(() => {
          if_widok_quizz1.value = true;
        }, 1000);
      }
    }
  };

  const wywolanie_sceny_koncowej = () => {
    console.log("wywołanie planszy wyboru etapu nr 2");
    if (ifFocusEmitGlobal.value === false) {
      emit("koniec-etap1");
    }

    if (ifFocusEmitGlobal.value === true) {
      emit("koniec-etap1-focus");
    }
  };
}

const koniecQuizu = () => {
  if (krok_gracz1_na_planszy.value < 15) {
    if_rzuc_kostka.value = true;
    if_ruch_gracza.value = false;
    const buttonRzutVis = new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(document.querySelector(".rzut1"));
      }, 300);
    });
  }

  if (krok_gracz1_na_planszy.value === 15) {
    if_rzuc_kostka.value = false;
    console.log("plansza win level!");
    emit("koniec-etap1");
  }
};

const koniecQuizuFocusOn = async () => {
  if (krok_gracz1_na_planszy.value < 15) {
    napisRuch.value.focus();
    setTimeout(() => {
      if_rzuc_kostka.value = true;
    }, 1000);

    setTimeout(() => {
      if_ruch_gracza.value = false;
      button_rzut.value.focus();
    }, 1200);
  }

  if (krok_gracz1_na_planszy.value === 15) {
    if_rzuc_kostka.value = false;
    napisRuch.value.focus();
    await nextTick();
    console.log("plansza win level focus!");
    ifFocusEmitGlobal.value = true;
    emit("koniec-etap1-focus");
  }
};

const koniecPulapki = () => {
  console.log("emmiter - krok do tyłu");
  console.log(krok_gracz1_na_planszy.value);

  //nowe roziazanie planszy zasadzka - różne efekty po wejściu na pole
  let oIlePol = trapType.value;
  console.log(oIlePol);

  if (krok_gracz1_na_planszy.value === 3) {
    krok_gracz1_na_planszy.value = krok_gracz1_na_planszy.value + 1;
    ruch_lokalny = ruch_lokalny + 1;
    console.log(krok_gracz1_na_planszy.value);
    pionek_left.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][0];
    pionek_top.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][1];
  }
  if (krok_gracz1_na_planszy.value === 6) {
    krok_gracz1_na_planszy.value = krok_gracz1_na_planszy.value - 2;
    ruch_lokalny = ruch_lokalny - 2;
    console.log(krok_gracz1_na_planszy.value);
    pionek_left.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][0];
    pionek_top.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][1];
  }
  if (krok_gracz1_na_planszy.value === 8) {
    krok_gracz1_na_planszy.value = krok_gracz1_na_planszy.value + 2;
    ruch_lokalny = ruch_lokalny + 2;
    console.log(krok_gracz1_na_planszy.value);
    pionek_left.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][0];
    pionek_top.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][1];
  }
  if (krok_gracz1_na_planszy.value === 11) {
    krok_gracz1_na_planszy.value = 0;
    ruch_lokalny = 0;
    console.log(krok_gracz1_na_planszy.value);
    pionek_left.value = 30;
    pionek_top.value = 330;
  }
  if (krok_gracz1_na_planszy.value === 14) {
    krok_gracz1_na_planszy.value = krok_gracz1_na_planszy.value - 1;
    ruch_lokalny = ruch_lokalny - 1;
    console.log(krok_gracz1_na_planszy.value);
    pionek_left.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][0];
    pionek_top.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][1];
  }

  // koniec tego rozwiązania

  if_ruch_gracza.value = false;
  if_rzuc_kostka.value = true;
};

const koniecPulapkiFocusOn = async () => {
  console.log("emmiter - krok do tyłu");
  console.log(krok_gracz1_na_planszy.value);

  //nowe roziazanie planszy zasadzka - różne efekty po wejściu na pole

  let oIlePol = trapType.value;
  console.log(oIlePol);

  if (krok_gracz1_na_planszy.value === 3) {
    krok_gracz1_na_planszy.value = krok_gracz1_na_planszy.value + 1;
    ruch_lokalny = ruch_lokalny + 1;
    console.log(krok_gracz1_na_planszy.value);
    pionek_left.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][0];
    pionek_top.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][1];
  }
  if (krok_gracz1_na_planszy.value === 6) {
    krok_gracz1_na_planszy.value = krok_gracz1_na_planszy.value - 2;
    ruch_lokalny = ruch_lokalny - 2;
    console.log(krok_gracz1_na_planszy.value);
    pionek_left.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][0];
    pionek_top.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][1];
  }
  if (krok_gracz1_na_planszy.value === 8) {
    krok_gracz1_na_planszy.value = krok_gracz1_na_planszy.value + 2;
    ruch_lokalny = ruch_lokalny + 2;
    console.log(krok_gracz1_na_planszy.value);
    pionek_left.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][0];
    pionek_top.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][1];
  }
  if (krok_gracz1_na_planszy.value === 11) {
    krok_gracz1_na_planszy.value = 0;
    ruch_lokalny = 0;
    console.log(krok_gracz1_na_planszy.value);
    pionek_left.value = 30;
    pionek_top.value = 330;
  }
  if (krok_gracz1_na_planszy.value === 14) {
    krok_gracz1_na_planszy.value = krok_gracz1_na_planszy.value - 1;
    ruch_lokalny = ruch_lokalny - 1;
    console.log(krok_gracz1_na_planszy.value);
    pionek_left.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][0];
    pionek_top.value =
      pozycje_pionka_gracza1[krok_gracz1_na_planszy.value - 1][1];
  }

  napisRuch.value.focus();

  setTimeout(() => {
    if_rzuc_kostka.value = true;
  }, 1000);

  setTimeout(() => {
    if_ruch_gracza.value = false;
    button_rzut.value.focus();
  }, 2000);
};

const odejmijSzanse = () => {
  console.log("odejmij szanse");
  ilosc_szans.value = ilosc_szans.value - 1;

  console.log("ilosc_szans:" + ilosc_szans.value);
  emit("odejmij-gwiazdke");

  if (ilosc_szans.value === 2) {
    if_szansa3.value = false;
  }
  if (ilosc_szans.value === 1) {
    if_szansa2.value = false;
  }
  if (ilosc_szans.value === 0) {
    if_szansa1.value = false;
    console.log("przegrałeś!!!");
    if_widok_quizz1.value = false;
    if (ifFocusEmitGlobal.value === false) {
      console.log("przegrana z myszki");
      emit("przegrana");
    }
    if (ifFocusEmitGlobal.value === true) {
      console.log("przegrana z focusa");
      emit("przegrana-focus");
    }
  }
};

function clickWithFocus(event) {
  event.preventDefault(); // Prevent native button click
  ifQuizzFocusOn.value = true;
  ifTrapFocusOn.value = true;
  ifFocusEmitGlobal.value = true;
  kostka_click();
}

function clickWithMouse() {
  ifFocusEmitGlobal.value = false;
  kostka_click();
}
</script>

<template>
  <h1 class="sr-only">Gra planszowa - poziom 1</h1>
  <div class="tlo2" role="img" aria-label="gra planszowa - poziom1"></div>
  <div class="ikona-start" role="img" alt=""></div>
  <div
    class="trasa"
    role="img"
    alt="grafika"
    aria-label="trasa gry zawierająca 16 pól"
  ></div>
  <div class="ikona-meta" role="img" alt=""></div>
  <div
    class="pionek1"
    :style="{ left: pionek_left + 'px', top: pionek_top + 'px' }"
    role="img"
    alt="ikona"
    aria-label="Pionek"
  ></div>
  <h3 class="szanse-napis">Szanse</h3>
  <div
    class="szansa1 szansa_ksztalt1"
    v-if="if_szansa1"
    role="img"
    alt="ikona"
    aria-label="Szansa 1"
  ></div>
  <div
    class="szansa2 szansa_ksztalt1"
    v-if="if_szansa2"
    role="img"
    alt="ikona"
    aria-label="Szansa 2"
  ></div>
  <div
    class="szansa3 szansa_ksztalt1"
    v-if="if_szansa3"
    role="img"
    alt="ikona"
    aria-label="Szansa 3"
  ></div>
  <div class="ruch1" ref="ruchGracza" v-if="if_ruch_gracza" tabindex="0">
    <p class="ruch-text">Ruch gracza</p>
  </div>
  <button
    ref="rzut1"
    class="rzut1 my-button anim1"
    v-if="if_rzuc_kostka"
    @click="clickWithMouse"
    @keydown.enter="clickWithFocus"
    role="button"
  >
    Rzuć kostką
  </button>
  <div
    class="kostka1"
    ref="oczkaKostka"
    tabindex="0"
    :class="{
      kostka1image1: isSet1,
      kostka1image2: isSet2,
      kostka1image3: isSet3,
      kostka1image4: isSet4,
      kostka1image5: isSet5,
      kostka1image6: isSet6,
    }"
    v-if="if_widok_kostki"
    role="img"
    alt="ikona widoku kostki"
    :aria-label="wyrzuconaWartoscKostki"
  ></div>
  <SceneTrap
    v-if="if_widok_pulapki"
    @koniec-pulapka="(if_widok_pulapki = false), koniecPulapki()"
    @koniec-pulapka-focus="(if_widok_pulapki = false), koniecPulapkiFocusOn()"
    :ifButtonOnFocusTrap="ifTrapFocusOn"
    :titleOfTrap="titleTrap"
    :textOfTrap="textTrap"
  />
  <SceneQuizz1
    v-if="if_widok_quizz1"
    @koniec-quizz="(if_widok_quizz1 = false), koniecQuizu()"
    @koniec-quizz-focus="
      (if_widok_quizz1 = false),
        (ifRzucKostkaButtonOnFocus = true),
        koniecQuizuFocusOn()
    "
    @odejmij-szanse="odejmijSzanse"
    msg="Hej"
    :miejsceNaPlanszy="krok_gracz1_na_planszy"
    :ifButtonOnFocusQuizz1="ifQuizzFocusOn"
  />
</template>

<style scoped>
.tlo2 {
  background-image: url("../assets/plansza_poziom1.png");
  background-size: 1920px 1080px;
  height: 1080px;
  width: 1920px;
  top: 0px;
  left: 0px;
  position: absolute;
}

.ikona-start {
  background-image: url("../assets/start_poziom1.png");
  background-size: 255px 305px;
  background-repeat: no-repeat;
  position: absolute;
  height: 305px;
  width: 255px;
  top: -18px;
  left: 23px;
  z-index: 0;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

.trasa {
  background-image: url("../assets/sama_plansza.png");
  background-size: 1280px 799px;
  background-repeat: no-repeat;
  position: absolute;
  height: 799px;
  width: 1280px;
  top: 155px;
  left: 0px;
  z-index: 0;
}

.ikona-meta {
  background-image: url("../assets/meta_poziom1.png");
  background-size: 267px 288px;
  background-repeat: no-repeat;
  position: absolute;
  height: 288px;
  width: 267px;
  top: 482px;
  left: 1133px;
  z-index: 0;
}

.pionek1 {
  background-image: url("../assets/pionek1.png");
  background-size: 116px 116px;
  background-repeat: no-repeat;
  height: 116px;
  width: 116px;
  position: absolute;
}

.szanse-napis {
  color: rgb(29, 56, 80);
  font-size: 45px;
  font-style: bold;
  font-weight: 600;
  font-family: "Proxima Nova", sans-serif;
  top: 255px;
  left: 1465px;
  height: 88px;
  width: 333px;
  position: absolute;
  z-index: 2;
}

.kostka1 {
  background-size: 250px 250px;
  background-repeat: no-repeat;
  left: 1549px;
  top: 687px;
  height: 250px;
  width: 250px;
  position: absolute;
  z-index: 2;
}

.kostka1:focus {
  outline: 5px solid #000000;
  outline-offset: 10px;
}

.kostka1image1 {
  background-image: url("../assets/kostka_1oczko.png");
}

.kostka1image2 {
  background-image: url("../assets/kostka_2oczka.png");
}

.kostka1image3 {
  background-image: url("../assets/kostka_3oczka.png");
}

.kostka1image4 {
  background-image: url("../assets/kostka_4oczka.png");
}

.kostka1image5 {
  background-image: url("../assets/kostka_5oczek.png");
}

.kostka1image6 {
  background-image: url("../assets/kostka_6oczek.png");
}

.rzut1 {
  color: rgb(255, 255, 255);
  font-size: 40px;
  font-style: bold;
  font-weight: 700;
  font-family: "Proxima Nova", sans-serif;
  background-image: url("../assets/rzut_przycisk.png");
  background-size: 333px 86px;
  background-repeat: no-repeat;
  top: 560px;
  left: 1502px;
  height: 88px;
  width: 333px;
  position: absolute;
  z-index: 2;
}

.rzut1:focus {
  outline: 5px solid #000000;
  outline-offset: 10px;
}

.ruch1 {
  color: rgb(29, 56, 80);
  font-size: 40px;
  font-style: bold;
  font-weight: 700;
  font-family: "Proxima Nova", sans-serif;
  background-size: 333px 86px;
  background-repeat: no-repeat;
  top: 560px;
  left: 1502px;
  height: 86px;
  width: 333px;
  position: absolute;
  z-index: 2;
  text-align: center;
}

.ruch1:focus {
  outline: 5px solid #000000;
  outline-offset: 10px;
}

.ruch-text {
  position: absolute;
  margin-top: 0.5em;
  margin-left: 1.35em;
}

.szansa_ksztalt1 {
  background-image: url("../assets/zycie1.png");
  background-size: 72px 72px;
  background-repeat: no-repeat;

  height: 72px;
  width: 72px;
  position: absolute;
  z-index: 2;
}

.szansa1 {
  top: 387px;
  left: 1530px;
}

.szansa2 {
  top: 387px;
  left: 1630px;
}

.szansa3 {
  top: 387px;
  left: 1730px;
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

/*anim1* The element to apply the animation to */
.anim1 {
  animation-name: example;
  animation-duration: 1s;
}
</style>

<script setup>
import { ref, onMounted,  useTemplateRef, nextTick  } from 'vue';
import { Quests2 } from '../lib/quests-source2.js';

defineOptions({
    inheritAttrs: false
})

const props = defineProps({
    miejsceNaPlanszy: Number,
    msg: String,
    ifButtonOnFocusQuizz2: Boolean
});

const ifButtonKoniecQuizzuOnFocus = ref(false)

const pytanieWidok = useTemplateRef('pytanie1')
const odpowiedzWidok = useTemplateRef('info')

onMounted(() => {
    // const elementToFocus = document.querySelector(".pytanie1")
    // if (elementToFocus && props.ifButtonOnFocusQuizz2) {
    //     elementToFocus.focus();
    // }
   if (props.ifButtonOnFocusQuizz2 === true) {
    pytanieWidok.value.focus();
  }
})

const emit = defineEmits(['koniec-quizz', 'koniec-quizz-focus',
    'odejmij-szanse']);

let nr_zestawu = Math.floor(Math.random() * 3);

console.log(props.msg);
console.log(props.miejsceNaPlanszy);

const quizz_assets_data = new Quests2();



const is_krzyzyk1 = ref(false);
const is_krzyzyk2 = ref(false);
const is_krzyzyk3 = ref(false);
const if_button_dalej = ref(false);
const if_odpowiedz_dobrze = ref(false);
const if_button_dalej_dobrze = ref(false);
const if_odpowiedz_zle = ref(false);
const if_button_dalej_zle = ref(false);
const czy_odpowiedz_poprawna = ref(false);
const czy_odpowiedz_zla = ref(false);

//czy zaznaczone
const zaznaczenieOdpowiedzi1 = ref(false)
const zaznaczenieOdpowiedzi2 = ref(false)
const zaznaczenieOdpowiedzi3 = ref(false)


const eksp1 = [
    "planszaQuizz1nr1",
    "planszaQuizz1nr2",
    "planszaQuizz1nr3",
    "planszaQuizz1nr4",
    "planszaQuizz1nr5",
    "planszaQuizz1nr6",
    "planszaQuizz1nr7",
    "planszaQuizz1nr8",
    "planszaQuizz1nr9",
    "planszaQuizz1nr10"
]

function zaznaczenie1() {
    console.log("Zaznaczenie 1");
    if (quizz_assets_data.pokaz_zadanie_2(props.miejsceNaPlanszy).odpowiedz_text[nr_zestawu][3] === 1) {
        console.log("Odpowiedź poprawna");
        czy_odpowiedz_poprawna.value = true;
        czy_odpowiedz_zla.value = false;
        zaznaczenieOdpowiedzi1.value = true
        zaznaczenieOdpowiedzi2.value = false
        zaznaczenieOdpowiedzi3.value = false

    } else {
        console.log("Odpowiedź zła");
        czy_odpowiedz_poprawna.value = false;
        czy_odpowiedz_zla.value = true;
        zaznaczenieOdpowiedzi1.value = true
        zaznaczenieOdpowiedzi2.value = false
        zaznaczenieOdpowiedzi3.value = false
    }
}

function zaznaczenie2() {
    console.log("Zaznaczenie 2");
    if (quizz_assets_data.pokaz_zadanie_2(props.miejsceNaPlanszy).odpowiedz_text[nr_zestawu][3] === 2) {
        console.log("Odpowiedź poprawna");
        czy_odpowiedz_poprawna.value = true;
        czy_odpowiedz_zla.value = false;
        zaznaczenieOdpowiedzi1.value = false
        zaznaczenieOdpowiedzi2.value = true
        zaznaczenieOdpowiedzi3.value = false
    } else {
        console.log("Odpowiedź zła");
        czy_odpowiedz_poprawna.value = false;
        czy_odpowiedz_zla.value = true;
        zaznaczenieOdpowiedzi1.value = false
        zaznaczenieOdpowiedzi2.value = true
        zaznaczenieOdpowiedzi3.value = false
    }
}

function zaznaczenie3() {
    console.log("Zaznaczenie 3");
    if (quizz_assets_data.pokaz_zadanie_2(props.miejsceNaPlanszy).odpowiedz_text[nr_zestawu][3] === 3) {
        console.log("Odpowiedź poprawna");
        czy_odpowiedz_poprawna.value = true;
        czy_odpowiedz_zla.value = false;
        zaznaczenieOdpowiedzi1.value = false
        zaznaczenieOdpowiedzi2.value = false
        zaznaczenieOdpowiedzi3.value = true
    } else {
        console.log("Odpowiedź zła");
        czy_odpowiedz_poprawna.value = false;
        czy_odpowiedz_zla.value = true;
        zaznaczenieOdpowiedzi1.value = false
        zaznaczenieOdpowiedzi2.value = false
        zaznaczenieOdpowiedzi3.value = true
    }
}

 async function sprawdzOdpowiedz() {
    console.log("Sprawdzam odpowiedź");
    if (czy_odpowiedz_poprawna.value) {
        console.log("Odpowiedź poprawna!!!!");
        if_odpowiedz_dobrze.value = true;
        if_button_dalej_dobrze.value = true;
        if_button_dalej.value = false;
        is_krzyzyk1.value = false;
        is_krzyzyk2.value = false;
        is_krzyzyk3.value = false
        const sound_dobrze = new Audio(new URL('../assets/Dobra_odp.mp3', import.meta.url).href);
        sound_dobrze.play();

        // const buttonVis = new Promise((resolve, reject) => {
        //     setTimeout(() => {
        //         resolve(document.querySelector(".info"))
        //     }, 300);
        // })
        // if (ifButtonKoniecQuizzuOnFocus.value === true) {
        //     buttonVis.then((res) => { res.focus() })
        // }

        await nextTick()

    
    
    console.log(odpowiedzWidok.value)
  
    
    if(odpowiedzWidok&&ifButtonKoniecQuizzuOnFocus.value===true){
      odpowiedzWidok.value.focus()
    }

    } else {
        console.log("Odpowiedź zła!!!!");
        if_odpowiedz_zle.value = true;
        if_button_dalej_zle.value = true;
        if_button_dalej.value = false;
        is_krzyzyk1.value = false;
        is_krzyzyk2.value = false;
        is_krzyzyk3.value = false;
        const sound_zle = new Audio(new URL('../assets/zla_odp.mp3', import.meta.url).href);
        sound_zle.play();
        emit('odejmij-szanse');

        // const buttonVis2 = new Promise((resolve, reject) => {
        //     setTimeout(() => {
        //         resolve(document.querySelector(".info"))
        //     }, 300);
        // })
        // if (ifButtonKoniecQuizzuOnFocus.value === true) {
        //     buttonVis2.then((res) => { res.focus() })
        // }

          await nextTick()
      console.log(odpowiedzWidok.value)
  
    
    if(odpowiedzWidok&&ifButtonKoniecQuizzuOnFocus.value===true){
      odpowiedzWidok.value.focus()
    }
    }
}

</script>
<template>
    <div class="planszaQuizz1 " :class="eksp1[quizz_assets_data.pokaz_zadanie_2(props.miejsceNaPlanszy).pytanie]"
        role="img" aria-label="pytanie">
        <h1 class="sr-only">Quizz</h1>
    </div>

    <!-- <div class="planszaQuizz1 " :class="eksp1[9]"></div> -->
    <p class="pytanie1" ref="pytanie1" tabindex="0" :aria-label="quizz_assets_data.pokaz_zadanie_2(props.miejsceNaPlanszy).tresc">{{ quizz_assets_data.pokaz_zadanie_2(props.miejsceNaPlanszy).tresc }}</p>
    <ul class="lista-odpowiedzi" role="list">
        <li>
            <div class="pojedyncza-odpowiedz" role="checkbox" tabindex="0" :aria-checked="zaznaczenieOdpowiedzi1"
                @click="is_krzyzyk1 = true,
                    is_krzyzyk2 = false,
                    is_krzyzyk3 = false,
                    if_button_dalej = true,
                    zaznaczenie1()" @keydown.enter="is_krzyzyk1 = true,
                        is_krzyzyk2 = false,
                        is_krzyzyk3 = false,
                        if_button_dalej = true,
                        zaznaczenie1()">

                <span class="krzyzykA" v-if="is_krzyzyk1" role="img" alt="zaznaczenie odpowiedzi"
                    aria-label="zaznaczona odpowiedź"></span>
                <span class="pole-zazn  anim1" aria-label="zaznacz odpowiedź 1"></span>

                <span class="odpowiedz anim1"
                    v-html="quizz_assets_data.pokaz_zadanie_2(props.miejsceNaPlanszy).odpowiedz_text[nr_zestawu][0]">
                </span>
            </div>
        </li>
        <li>
            <div class="pojedyncza-odpowiedz" role="checkbox" tabindex="0" :aria-checked="zaznaczenieOdpowiedzi2"
                @click="is_krzyzyk2 = true,
                    is_krzyzyk1 = false,
                    is_krzyzyk3 = false,
                    if_button_dalej = true,
                    zaznaczenie2()" @keydown.enter="is_krzyzyk2 = true,
                        is_krzyzyk1 = false,
                        is_krzyzyk3 = false,
                        if_button_dalej = true,
                        zaznaczenie2()">
                <span class="krzyzykA" role="img" v-if="is_krzyzyk2" alt="zaznaczenie odpowiedzi"
                    aria-label="zaznaczona odpowiedź"></span>
                <span class="pole-zazn anim1" aria-label="zaznacz odpowiedź 2"></span>

                <span class="odpowiedz anim1"
                    v-html="quizz_assets_data.pokaz_zadanie_2(props.miejsceNaPlanszy).odpowiedz_text[nr_zestawu][1]">
                </span>
            </div>
        </li>
        <li>
            <div class="pojedyncza-odpowiedz" role="checkbox" tabindex="0" :aria-checked="zaznaczenieOdpowiedzi3"
                @click="is_krzyzyk3 = true,
                    is_krzyzyk1 = false,
                    is_krzyzyk2 = false,
                    if_button_dalej = true,
                    zaznaczenie3()" @keydown.enter="is_krzyzyk3 = true,
                        is_krzyzyk1 = false,
                        is_krzyzyk2 = false,
                        if_button_dalej = true,
                        zaznaczenie3()">
                <span class="krzyzykA" v-if="is_krzyzyk3" role="img" alt="zaznaczenie odpowiedzi"
                    aria-label="zaznaczona odpowiedź"></span>
                <span class="pole-zazn anim1" aria-label="zaznacz odpowiedź 3"></span>

                <span class="odpowiedz anim1"
                    v-html="quizz_assets_data.pokaz_zadanie_2(props.miejsceNaPlanszy).odpowiedz_text[nr_zestawu][2]">
                </span>
            </div>
        </li>
    </ul>

    <button class="button-dalej my-button" v-if="if_button_dalej" @click="sprawdzOdpowiedz()"
        @keydown.enter="ifButtonKoniecQuizzuOnFocus = true; sprawdzOdpowiedz" role="button">Sprawdź
        odpowiedź</button>
    <div class="plansza-dobrze" v-if="if_odpowiedz_dobrze">
        <div class="info" ref="info" tabindex="0" aria-label="Brawo! Prawidłowa odpowiedź.">
            <p class="naglowek-after-quizz naglowek-dobrze">Brawo!</p>
            <p class="napis-odpowiedz napis-dobrze">Prawidłowa odpowiedź.</p>
        </div>
    </div>
    <button class="button-dalej-dobrze my-button anim1" v-if="if_button_dalej_dobrze" @click="if_odpowiedz_dobrze = false,
        if_button_dalej_dobrze = false,
        $emit('koniec-quizz')" @keydown.enter="if_odpowiedz_dobrze = false,
            if_button_dalej_dobrze = false, $emit('koniec-quizz-focus')" role="button">Dalej</button>
    <div class="plansza-zle" v-if="if_odpowiedz_zle">
         <div class="info" ref="info" tabindex="0" aria-label="Źle! Błędna odpowiedź. Tracisz jedną szansę">
        <p class="naglowek-after-quizz naglowek-zle">Źle!</p>
        <p class="napis-odpowiedz napis-zle">Błędna odpowiedź.</p>
        </div>
    </div>
    <button class="button-dalej-dobrze my-button anim1" v-if="if_button_dalej_zle" @click="if_odpowiedz_zle = false,
        if_button_dalej_zle = false,
        $emit('koniec-quizz')" @keydown.enter="if_odpowiedz_dobrze = false,
            if_button_dalej_dobrze = false, $emit('koniec-quizz-focus')" role="button">Dalej</button>

</template>

<style scoped>
.planszaQuizz1 {

    background-size: 1411px 896px;
    background-repeat: no-repeat;
    height: 896px;
    width: 1411px;
    position: absolute;
    left: 0px;
    top: 100px;
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

.planszaQuizz1nr1 {
    background-image: url("../assets/pytanie_puste.png");
}

.planszaQuizz1nr2 {
    background-image: url("../assets/pytanie_puste.png");
}

.planszaQuizz1nr3 {
    background-image: url("../assets/pytanie_puste.png");
}

.planszaQuizz1nr4 {
    background-image: url("../assets/pytanie_puste.png");
}

.planszaQuizz1nr5 {
    background-image: url("../assets/pytanie_puste.png");
}

.planszaQuizz1nr6 {
    background-image: url("../assets/pytanie_puste.png");
}

.planszaQuizz1nr7 {
    background-image: url("../assets/pytanie_puste.png");
}

.planszaQuizz1nr8 {
    background-image: url("../assets/pytanie_puste.png");
}

.planszaQuizz1nr9 {
    background-image: url("../assets/pytanie_puste.png");
}

.planszaQuizz1nr10 {
    background-image: url("../assets/pytanie_puste.png");
}

.pytanie1 {
    color: rgb(29, 56, 80);
    opacity: 1;
    /* tutaj na potrzeby czytnika można dać 0 */
    font-size: 42px;
    font-style: bold;
    font-weight: 700;
    font-family: "Proxima Nova", sans-serif;
    position: absolute;
    white-space: nowrap;
    top: 290px;
    left: 190px;
    padding: 20px;
}

.pytanie1:focus {
      outline: 2px solid #000000 !important;
}

ul {
    display: block;
    margin-block-start: 1em;
    margin-block-end: 1em;
}

li {
    margin-bottom: 20px;
}

.lista-odpowiedzi {
    position: absolute;
    left: 180px;
    top: 435px;
    list-style: none;
}

.pojedyncza-odpowiedz {
    display: flex;
    align-items: center;
    padding: .5rem 1rem;
}

.pojedyncza-odpowiedz:focus {
    outline: 2px solid #000000 !important;
}

.pole-zazn {
    /* background-image: url("../assets/kratka.png");
    background-size: 50px 50px;
    background-repeat: no-repeat; */
    border: rgb(29, 56, 80) solid 2px;
    height: 81px;
    width: 81px;
    margin-right: 1rem;
    /* position: absolute; */
}

.pole-zazn:hover {
    cursor: pointer;
}

.pole-zazn:focus {
    /* outline: thick double #08e926 !important; */
     outline: 2px solid #000000 !important;
}

.pole1 {
    left: 200px;
    top: 455px;
}

.pole1:hover {
    cursor: pointer;
}

.pole2 {
    left: 200px;
    top: 585px;
}

.pole2:hover {
    cursor: pointer;
}

.pole3 {
    left: 200px;
    top: 715px;
}

.pole3:hover {
    cursor: pointer;
}

.krzyzykA {
    background-image: url("../assets/krzyzyk1.png");
    background-size: 73px 73px;
    background-repeat: no-repeat;
    height: 73px;
    width: 73px;
    left: 62px;
    position: absolute;
    z-index: 1;
}

/* nieużywane klasy do usunięcia -  początek*/
.krzyzyk {
    background-image: url("../assets/krzyzyk1.png");
    background-size: 73px 73px;
    background-repeat: no-repeat;
    height: 73px;
    width: 73px;
    position: absolute;
    visibility: hidden;
    z-index: 1;
}

.krzyzyk1 {
    top: 15px;
    left: 62px;
    visibility: visible;
}

.krzyzyk2 {
    top: 135px;
    left: 62px;
    visibility: visible;
}

.krzyzyk3 {
    top: 255px;
    left: 62px;
    visibility: visible;
}

/* nieużywane klasy do usunięcia - koniec*/

.odpowiedz {
    color: rgb(29, 56, 80);
    font-size: 42px;
    font-style: bold;
    font-weight: 700;
    font-family: "Proxima Nova", sans-serif;

    width: 1000px;
    /* position: absolute; */

}

.odpowiedz1 {
    top: 423px;
    left: 300px;
}

.odpowiedz2 {
    top: 552px;
    left: 300px;
}

.odpowiedz3 {
    top: 682px;
    left: 300px;
}

.button-dalej {
    background-image: url("../assets/sprawdz_odpwowiedz_button1.png");
    color: rgb(255, 255, 255);
    font-size: 37px;
    font-style: bold;
    font-weight: 500;
    font-family: "Proxima Nova", sans-serif;
    background-size: 394px 87px;
    background-repeat: no-repeat;
    top: 838px;
    left: 300px;
    height: 87px;
    width: 394px;
    position: absolute;
    z-index: 2;
}

.button-dalej:hover {
    cursor: pointer;
}

.button-dalej:focus {

    outline: 5px solid #000000 !important;
  outline-offset: 10px;
}

.plansza-dobrze {
    background-image: url("../assets/KOMUNIKAT_dobra_odp.png");
    background-size: 1212px 533px;
    background-repeat: no-repeat;
    height: 533px;
    width: 1212px;
    position: absolute;
    left: 83px;
    top: 275px;
}

.info{
  height: 273px;
  width: 812px;
  position: absolute;
  left: 200px;
  top: 50px;
}

.info:focus{
  outline: 2px solid #ffffff !important;
}

.plansza-zle {
    background-image: url("../assets/KOMUNIKAT_zla_odp.png");
    background-size: 1212px 533px;
    background-repeat: no-repeat;
    height: 533px;
    width: 1212px;
    position: absolute;
    left: 83px;
    top: 275px;
}

.naglowek-after-quizz {
    color: rgb(255, 255, 255);
    font-size: 100px;
    font-style: bold;
    font-weight: 600;
    font-family: "Proxima Nova", sans-serif;
   top: -90px;

    height: 88px;
    width: 333px;
    position: absolute;
    z-index: 2;
}

.naglowek-dobrze {
     left: 230px;
}

.naglowek-zle {
      left: 330px;
}

.napis-odpowiedz {
    color: rgb(255, 255, 255);
    font-size: 70px;
    font-style: bold;
    font-weight: 400;
    font-family: "Proxima Nova", sans-serif;
    white-space: nowrap;
   top: 80px;

    height: 88px;
    width: 333px;
    position: absolute;
    z-index: 2;
}

.napis-dobrze {
  left: 50px;
}

.napis-zle {
  left:130px;
}

.button-dalej-dobrze {
    /* background-image: url("../assets/przycisk_dalej_imie.png"); */
    color: rgb(29, 56, 80);
    font-size: 80px;
    font-style: bold;
    font-weight: 700;
    font-family: "Proxima Nova", sans-serif;
    background-size: 301px 117px;
    background-position: -2px -1px;
    background-repeat: no-repeat;
    top: 630px;
    left: 535px;
    height: 117px;
    width: 301px;
    position: absolute;
    z-index: 2;
}

.button-dalej-dobrze:hover {
    cursor: pointer;
}

.button-dalej-dobrze:focus {
   outline: 5px solid #ffffff !important;
  outline-offset: 10px;
}

/* The animation code */
@keyframes example {

    /* from {background-color: red;}
  to {background-color: yellow;} */
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
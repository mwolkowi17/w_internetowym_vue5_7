<script setup>
import { ref, onMounted, useTemplateRef } from 'vue';
defineEmits(['jeszcze-raz', 'jeszcze-raz-focus', 'koniec-gry']);

const props = defineProps({
    ifButtonOnFocusSceneLose: Boolean
});

const infoEnd = useTemplateRef('info-end2')

onMounted(() => {
    // const elementToFocus = document.querySelector(".info-end2")
    // if (elementToFocus&&props.ifButtonOnFocusSceneLose===true) {
    //     elementToFocus.focus();
    // }
    infoEnd.value.focus()
})

const przegrana_sound = new Audio(new URL('../assets/przegrana_gra.mp3', import.meta.url).href);
przegrana_sound.play();
</script>

<template>
    <div class="plansza-lose">
        <div class="info-end2" ref="info-end2" tabindex="0" aria-label="Niestety przegrałeś. Chcesz spróbować jeszcze raz?">
            <h2 class="naglowek">Przegrana.</h2>
            <h2 class="napis">Chcesz spróbować jeszcze raz?</h2>
        </div>
    </div>
    <button class="gram-jeszcze-1 my-button anim1" @click="$emit('jeszcze-raz')"
        @keydown.enter="$emit('jeszcze-raz-focus')" role="button">Zagraj jeszcze raz</button>
    <button class="zakoncz-gre my-button anim1" @click="$emit('koniec-gry')" role="button">Zakończ grę</button>
</template>

<style scoped>
.plansza-lose {
    background-image: url("../assets/przegrana_plansza.png");
    background-size: 1920px 1080px;
    height: 1080px;
    width: 1920px;
    top: 0px;
    left: 0px;
    position: absolute;
    z-index: 2;
}

.info-end2 {
    position: absolute;
    height: 400px;
    width: 1200px;
    top: 200px;
    left: 350px
}

.info-end2:focus {
    outline: 2px solid #ffffff;
}

.naglowek {
    color: rgb(255, 255, 255);
    font-size: 100px;
    font-style: bold;
    font-weight: 600;
    font-family: "Proxima Nova", sans-serif;
    top: 0px;
    left: 355px;
    height: 88px;
    width: 333px;
    position: absolute;
    z-index: 2;
    white-space: nowrap;
}

.napis {
    color: rgb(255, 255, 255);
    font-size: 70px;
    font-style: bold;
    font-weight: 400;
    font-family: "Proxima Nova", sans-serif;
    white-space: nowrap;
    top: 200px;
    left: 120px;
    height: 88px;
    width: 333px;
    position: absolute;
    z-index: 2;
}

.gram-jeszcze-1 {
    /* background-image: url("../assets/gram_jeszcze_raz_button.png"); */
    color: rgb(29, 56, 80);
    font-size: 60px;
    font-style: bold;
    font-weight: 700;
    font-family: "Proxima Nova", sans-serif;
    background-size: 583px 192px;
    background-position: -1px -2px;
    position: absolute;
    top: 650px;
    left: 300px;
    width: 583px;
    height: 192px;
    z-index: 3;
    border: 4px solid rgb(0, 187, 255);
}

.gram-jeszcze-1:hover {
    cursor: pointer;
}

.gram-jeszcze-1:focus {
      outline: 5px solid #ffffff;
    outline-offset: 10px;
}

.zakoncz-gre {
    /* background-image: url("../assets/zakoncz_gre_button.png"); */
    color: rgb(29, 56, 80);
    font-size: 60px;
    font-style: bold;
    font-weight: 700;
    font-family: "Proxima Nova", sans-serif;
    background-size: 583px 192px;
    background-position: -1px -2px;
    position: absolute;
    top: 650px;
    left: 1020px;
    width: 583px;
    height: 192px;
    z-index: 3;
    border: 4px solid rgb(0, 187, 255);
}

.zakoncz-gre:hover {
    cursor: pointer;
}

.zakoncz-gre:focus {
    outline: 5px solid #ffffff;
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
<script setup>
import { onMounted } from 'vue';
defineEmits(['wybor-levelu1', 'wybor-levelu1-focus']);

const props = defineProps({
    ifButtonOnFocusLevelOne: Boolean
});

onMounted(() => {
    const elementToFocus = document.querySelector(".level-one-button")
    if (elementToFocus && props.ifButtonOnFocusLevelOne === true) {
        elementToFocus.focus();
    }

})
</script>

<template>
    <div class="tlo-level1" role="img" aria-label="Wybór poziomu">
        <h1 class="sr-only">Wybór poziomu</h1>
    </div>
    <button class="level-one-button my-button" @click="$emit('wybor-levelu1')"
        @keydown.enter="$emit('wybor-levelu1-focus')" role="button">Poziom 1</button>
    <button class="level-two-button" role="img" aria-label="poziom 2 niedostępny" aria-disabled="true" disabled>Poziom
        2</button>

</template>

<style scoped>
.tlo-level1 {
    background-image: url("../assets/plansza_poziom1_wybor.png");
    background-size: 1920px 1080px;
    height: 1080px;
    width: 1920px;
    top: 0px;
    left: 0px;
    position: absolute;
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

.level-one-button {
    color: rgb(29, 56, 80);
    font-size: 100px;
    font-style: bold;
    font-weight: 700;
    font-family: "Proxima Nova", sans-serif;
    background-size: 595px 200px;
    background-position: -1px -1px;
    position: absolute;
    top: 438px;
    left: 220px;
    width: 595px;
    height: 200px;
    border: 4px solid rgb(0, 187, 255);
}

.level-one-button:hover {
    cursor: pointer;
}

.level-one-button:focus {
    outline: 5px solid white;
    outline-offset: 10px;
}

.level-two-button {
    color: rgb(29, 56, 80);
    font-size: 100px;
    font-style: bold;
    font-weight: 700;
    font-family: "Proxima Nova", sans-serif;
    background-size: 595px 200px;
    background-position: -2px -1px;
    position: absolute;
    top: 438px;
    left: 1063px;
    width: 595px;
    height: 200px;
    border: 4px solid rgb(0, 187, 255);
}
</style>
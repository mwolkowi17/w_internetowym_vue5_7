<script setup>
import { ref, onMounted, useTemplateRef, nextTick } from 'vue';

defineEmits(['koniec-pulapka', 'koniec-pulapka-focus']);

defineOptions({
  inheritAttrs: false
})

const props = defineProps({
  ifButtonOnFocusTrap: Boolean,
  titleOfTrap:String,
  textOfTrap:String
})

const trapWidok = useTemplateRef('info-trap')

onMounted(() => {
  // const elementToFocus = document.querySelector(".info-trap")
  // if (elementToFocus&&props.ifButtonOnFocusTrap===true) {
  //   elementToFocus.focus();
  // }
  if (props.ifButtonOnFocusTrap === true) {
    trapWidok.value.focus()
  }

})

function koniecPulapki() {

  // Emit the event to notify the parent component that the trap has ended
  console.log("Koniec pulapki");

}

const textAria=props.titleOfTrap+props.textOfTrap


</script>
<template>
  <div class="pulapka1">
    <div class="info-trap" ref="info-trap" tabindex="0" :aria-label="props.titleOfTrap+props.textOfTrap">
      <h2 class="naglowek">{{ props.titleOfTrap }}</h2>
      <h3 class="napis">{{props.textOfTrap}}</h3>
    </div>
  </div>
  <button class="przycisk_pulapka1 my-button anim1" @click="$emit('koniec-pulapka'); koniecPulapki()"
    @keydown.enter="$emit('koniec-pulapka-focus')" role="button">Dalej</button>
</template>
<style scoped>
.pulapka1 {
  background-image: url("../assets/zasadzka.png");
  background-size: 1212px 533px;
  background-repeat: no-repeat;
  height: 533px;
  width: 1212px;
  position: absolute;
  left: 100px;
  top: 250px;
  position: absolute;
  left: 100px;
  top: 250px;
    display: flex;
  height: 800px;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}

.info-trap {
  height: 256px;
  width: 912px;
  /* position: absolute;
  left: 200px;
  top: 50px; */
    margin-top: 3em;
   display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.info-trap:focus {
  outline: 2px solid #ffffff !important;
}

.naglowek {
  color: rgb(255, 255, 255);
  font-size: 100px;
  font-style: bold;
  font-weight: 600;
  font-family: "Proxima Nova", sans-serif;
  /* top: -70px;
  left: 150px;
  ;
  height: 88px;
  width: 333px;
  position: absolute;
  z-index: 2; */
   margin-bottom: 0em;
 margin-top: 0.2em;
  height: 88px;
  width: 333px;
  display: flex;
  justify-content: center;
  z-index: 2;

}

.napis {
  color: rgb(255, 255, 255);
  font-size: 70px;
  font-style: bold;
  font-weight: 400;
  font-family: "Proxima Nova", sans-serif;
  white-space: nowrap;
  /* top: 80px;
  ;
  left: 60px;
  height: 88px;
  width: 333px;
  position: absolute;
  z-index: 2; */
  margin-top: 0.5em;
  margin-bottom: 0.6em;
  height: 88px;
  width: 333px;

  z-index: 2;
    display: flex;
  justify-content: center;
}

.przycisk_pulapka1 {
  /* background-image: url("../assets/przycisk_dalej_imie.png"); */
  color: rgb(29, 56, 80);
  font-size: 80px;
  font-style: bold;
  font-weight: 700;
  font-family: "Proxima Nova", sans-serif;
  background-size: 301px 117px;
  background-position: -1px -2px;
  background-repeat: no-repeat;
  height: 117px;
  width: 301px;
  position: absolute;
  left: 545px;
  top: 590px;
  border: 4px solid rgb(0, 187, 255);

}

.przycisk_pulapka1:hover {
  cursor: pointer;
}

.przycisk_pulapka1:focus {
  /* outline: thick double #08e926; */
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
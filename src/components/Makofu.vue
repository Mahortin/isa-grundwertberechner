<script setup lang="ts">
defineProps<{
  msg: string
}>()
import { ref } from 'vue'
import { reactive, computed } from 'vue'

const mut = ref(8)
const klugheit = ref(8)
const intuition = ref(8)

const verborgenesErkennen = ref(Math.round((mut.value + klugheit.value + intuition.value) / 3))
const weisheit = ref(klugheit.value - mut.value)


function setMut(e, v = +e.target.value) {
  mut.value = v
  calcEverything()
}

function setKlugheit(e, v = +e.target.value) {
  klugheit.value = v
  calcEverything()
}

function setIntuition(e, v = +e.target.value) {
  intuition.value = v
  calcEverything()
}


function calcEverything(){
  calcWeisheit()
  calcVerborgenesErkennen()
  publishedBooksMessage.value = hasPublishedBooks()
}

function calcWeisheit() {
  weisheit.value = (klugheit.value - mut.value)
}
function calcVerborgenesErkennen() {
  verborgenesErkennen.value = Math.round((mut.value + klugheit.value + intuition.value) / 3)
}

</script>

<template>
  <input type="number" :value="mut" @change="setMut"> Mut 
  <input type="number" :value="klugheit" @change="setKlugheit"> Klugheit
  <input type="number" :value="intuition" @change="setIntuition"> Intuition

  <p>Weisheit: {{ weisheit }}</p>
  <p>Verborgenes erkennen: {{ verborgenesErkennen }}</p>

</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>

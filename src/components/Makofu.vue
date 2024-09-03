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

const attributes = ref([
  {name: 'Mut', shortName: 'MU', value: 8},
  {name: 'Klugheit', shortName: 'KL', value: 10},
  {name: 'Intuition', shortName: 'IN', value: 100}
])


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
  <div v-for="attribute in attributes">
    <label>{{attribute.name}}</label>
    <label>{{attribute.shortName}}</label>
    <label>{{attribute.value}}</label>
    <input type="number" min="8" max="16" v-model="attribute.value" 
  </div>

  <input type="number" min="8" max="16" :value="mut" @change="setMut"> Mut 
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

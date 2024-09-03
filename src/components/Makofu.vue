<script setup lang="ts">
defineProps<{
  msg: string
}>()
import { ref } from 'vue'
import { reactive, computed } from 'vue'

const works = ref(0)
const mut = ref(8)
const klugheit = ref(8)
const intuition = ref(8)

const verborgenesErkennen = ref(Math.round((mut.value + klugheit.value + intuition.value) / 3))
const weisheit = ref(klugheit.value - mut.value)

const attributes = reactive([
  {name: 'Mut', shortName: 'MU', value: 8},
  {name: 'Klugheit', shortName: 'KL', value: 10},
  {name: 'Intuition', shortName: 'IN', value: 100}
])


function setMut(e, v = +e.target.value) {
  mut.value = v
  calcEverything()
}

function setKlugheit(e, v = +e.target.value) {
  console.log(`setKlugheit triggered`);
  klugheit.value = v
  calcEverything()
}

function setIntuition(e, v = +e.target.value) {
  intuition.value = v
  calcEverything()
}


function calcEverything(shortName){
  var isaAttribute = attributes.find((attribute) => attribute.shortName === shortName)
  calcEverythingBackup()
  var test = getAttribute(shortName)
}

function calcWeisheit() {
  var isaKL = attributes.find((attribute) => attribute.shortName === "KL")
  var isaMU = attributes.find((attribute) => attribute.shortName === "MU")
  weisheit.value = isaKL.value - isaMU.value
}

function calcVerborgenesErkennen() {
  var isaMU = attributes.find((attribute) => attribute.shortName === "MU")
  var isaKL = attributes.find((attribute) => attribute.shortName === "KL")
  var isaIN = attributes.find((attribute) => attribute.shortName === "IN")
  verborgenesErkennen.value = Math.round((isaMU.value + isaKL.value + isaIN.value) / 3)
}

function getAttribute(shortName) {
  return attributes.find((attribute) => attribute.shortName === shortName)
}

</script>

<template>
  <div v-for="attribute in attributes">
    <label>{{attribute.name}}</label>
    <label>{{attribute.shortName}}</label>
    <label>{{attribute.value}}</label>
    <input type="number" min="8" max="16" @change="calcEverything(attribute.shortName)" v-model.number="attribute.value" 
  </div>

  <p>Weisheit: {{ weisheit }}</p>
  <p>Verborgenes erkennen: {{ verborgenesErkennen }}</p>
  <p>Works: {{ works }}</p>

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

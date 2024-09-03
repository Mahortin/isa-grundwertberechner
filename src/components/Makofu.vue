<script setup lang="ts">
defineProps<{
  msg: string
}>()
import { ref } from 'vue'

const mut = ref(8)
const klugheit = ref(12)

function setMut(e, v = +e.target.value) {
  mut.value = v
  klugheit.value = v * (9 / 5) + 32
}

function setKlugheit(e, v = +e.target.value) {
  klugheit.value = v
  mut.value = (v - 32) * (5 / 9)
}
</script>

<template>
  <input type="number" :value="mut" @change="setMut"> Mut =
  <input type="number" :value="klugheit" @change="setKlugheit"> Klugheit
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

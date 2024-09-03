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
  {key: 'MU',name: 'Mut', value: 8},
  {key: 'KL', name: 'Klugheit', value: 8},
  {key: 'IN', name: 'Intuition', value: 8},
  {key: 'CH', name: 'Charisma', value: 8},
  {key: 'FF', name: 'Fingerfertigkeit', value: 8},
  {key: 'GE', name: 'Gewandheit', value: 8},
  {key: 'KO', name: 'Konstitution', value: 8},
  {key: 'KK', name: 'Körperkraft', value: 8}
])

const skillGroups = reactive([
  {key: 'koerper', name: 'Körper', skills: [
    {key: 'akrobatik', name: 'Akrobatik', mapIsaAttributes: ['MU', 'GE', 'KK'], value: 0},
    {key: 'verborgenesErkennen', name: 'Verborgenes Erkennen', mapIsaAttributes: ['MU', 'KL', 'IN'], value: 0}
  ]},
  {key: 'gesellschaft', name: 'Gesellschaft', skills: [
    {key: 'einschuechtern', name: 'Einschüchtern', mapIsaAttributes: ['MU', 'CH', 'KK'], value: 0},
    {key: 'handel', name: 'Handel', mapIsaAttributes: ['KL', 'IN', 'CH'], value: 0}
  ]}
])

function calcEverything(key){
  var isaAttribute = attributes.find((attribute) => attribute.key === key)
  calcReferencedSkills(key)
}

function calcAllSkills() {
  skills.forEach(skill => {
  })
}

function calcReferencedSkills(attributeKey) {
  skillGroups.forEach(group => {
    group.skills
      .filter(skill => skill.mapIsaAttributes.includes(attributeKey))
      .forEach(skill => {
        calcSkill(skill)
    })
  })

}

function calcSkill(skill) {
  var sum = 0
  for (var index in skill.mapIsaAttributes) {
    var attribute = attributes.find((attribute) => attribute.key === skill.mapIsaAttributes[index])
    sum += attribute.value
  }
  skill.value = Math.round(sum/3)
}

function getAttribute(key) {
  return attributes.find((attribute) => attribute.key === key)
}

</script>

<template>
  <div v-for="attribute in attributes">
    <label>{{attribute.name}}</label>
    <label>{{attribute.key}}</label>
    <label>{{attribute.value}}</label>
    <input type="number" min="8" max="16" @change="calcEverything(attribute.key)" v-model.number="attribute.value" 
  </div>

  <div v-for="group in skillGroups">
    <label>{{group.name}}</label>
      <div v-for="skill in group.skills">
        <label>{{skill.key}}</label>
        <label>{{skill.name}}</label>
        <label>{{skill.mapIsaAttributes}}</label>
        <label>{{skill.value}}</label>
      </div>
  </div>

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

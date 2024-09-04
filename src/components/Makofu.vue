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
  {key: 'MU',name: 'Mut', value: 8, increased: false},
  {key: 'KL', name: 'Klugheit', value: 8, increased: false},
  {key: 'IN', name: 'Intuition', value: 8, increased: false},
  {key: 'CH', name: 'Charisma', value: 8, increased: false},
  {key: 'FF', name: 'Fingerfertigkeit', value: 8, increased: false},
  {key: 'GE', name: 'Gewandheit', value: 8, increased: false},
  {key: 'KO', name: 'Konstitution', value: 8, increased: false},
  {key: 'KK', name: 'Körperkraft', value: 8, increased: false}
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

function increase(attribute) {
  attribute.increased = !attribute.increased
}

</script>

<template>
<div class="container">
    <!-- Column for Attributes -->
    <div class="column">
      <h2>Attributes</h2>
      <div v-for="attribute in attributes" :key="attribute.key" class="attribute-item">
        <div class="attribute-info">
          <span class="attribute-key">{{ attribute.key }}</span>
          <span class="attribute-name">{{ attribute.name }}</span>
          <span class="attribute-value">{{ attribute.value }}</span>
          
        </div>
        <input 
          type="number" 
          min="8" 
          max="16" 
          @change="calcEverything(attribute.key)" 
          v-model.number="attribute.value"
          class="attribute-input"
        />
        <label color="[attribute.increased ? 'green' ]" @click="increase(attribute)">{{attribute.increased}}</label>
        <button :class="[attribute.increased ? 'active-button' : '']" @click="increase(attribute)">+</button>
      </div>
    </div>

    <!-- Column for Skill Groups -->
    <div class="column">
      <h2>Skill Groups</h2>
      <div v-for="group in skillGroups" :key="group.name" class="skill-group">
        <h3>{{ group.name }}</h3>
        <div v-for="skill in group.skills" :key="skill.key" class="skill-item">
          <div class="skill-info">
            <span class="skill-name">{{ skill.name }}</span>
            <span class="skill-attributes">{{ skill.mapIsaAttributes }}</span>
            <span class="skill-value">{{ skill.value }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<style scoped>
.container {
  display: flex;
  justify-content: space-between;
}

.column {
  width: 48%;
}

.attribute-item, .skill-item {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  white-space: nowrap; /* Prevent line breaks within the items */
}

.attribute-info, .skill-info {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.attribute-name, .attribute-key, .attribute-value,
.skill-key, .skill-name, .skill-attributes, .skill-value {
  margin-right: 10px;
  flex-shrink: 0; /* Prevent shrinking */
  overflow: hidden;
  text-overflow: ellipsis; /* Truncate text with ellipsis if it overflows */
}

.attribute-input {
  flex-shrink: 0;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 8px; /* Softer edges with rounded corners */
  background-color: #f8f2e8; /* Light gray background */
  color: #333; /* Darker font color */
  font-size: 16px;
  cursor: pointer;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* Soft shadow for depth */
  transition: background-color 0.3s ease, box-shadow 0.3s ease; /* Smooth transition effects */
}

/* Active state: Change only the background color */
button.active-button {
  background-color: #713604; /* Modern blue background for active state */
  color: white; /* Change the font color to white when active */
}

/* Hover state: Slight background change and shadow lift */
button:hover {
  background-color: #e0e0e0; /* Light gray on hover for normal buttons */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15); /* Increase shadow for hover effect */
}

/* Hover state for active button */
button.active-button:hover {
  background-color: #783c0b; /* Slightly darker blue on hover */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15); /* Increase shadow for hover effect */
}

h3, h4 {
  margin-bottom: 15px;
}

</style>

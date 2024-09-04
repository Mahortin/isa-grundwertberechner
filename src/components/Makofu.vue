<script setup lang="ts">
defineProps<{
  msg: string
}>()
import { ref, reactive, computed } from 'vue'

const showOnlyIncreasedSkills = ref(false)
const myTest = ref(false)

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
  {key: 'koerper', name: 'Körper', increasedSkills: 0, skills: [
    {key: 'akrobatik', name: 'Akrobatik', mapIsaAttributes: ['MU', 'GE', 'KK'], value: 8, increased: false},
    {key: 'athletik', name: 'Athletik', mapIsaAttributes: ['GE', 'KO', 'KK'], value: 8, increased: false},
    {key: 'verborgenesErkennen', name: 'Verborgenes Erkennen', mapIsaAttributes: ['MU', 'KL', 'IN'], value: 8, increased: false}
  ]},
  {key: 'natur', name: 'Natur', increasedSkills: 0, skills: [
    {key: 'faehrtensuche', name: 'Fährtensuche', mapIsaAttributes: ['KL', 'IN', 'KO'], value: 8, increased: false},
    {key: 'fischenAngeln', name: 'Fischen & Angeln', mapIsaAttributes: ['IN', 'FF', 'KK'], value: 8, increased: false},
    {key: 'himmelskunde', name: 'Himmelskunde', mapIsaAttributes: ['KL', 'IN', 'IN'], value: 8, increased: false}
  ]},
  {key: 'gesellschaft', name: 'Gesellschaft', increasedSkills: 0, skills: [
    {key: 'einschuechtern', name: 'Einschüchtern', mapIsaAttributes: ['MU', 'CH', 'KK'], value: 8, increased: false},
    {key: 'handel', name: 'Handel', mapIsaAttributes: ['KL', 'IN', 'CH'], value: 8, increased: false}, 
    {key: 'schauspielerei', name: 'Schauspielerei', mapIsaAttributes: ['MU', 'KL', 'CH'], value: 8, increased: false}
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
    group.increasedSkills = group.skills.filter((skill) => skill.increased === true).length
    })
  })
}

function calcSkill(skill) {
  var sum = 0
  var increasedAttributes = 0
  for (var index in skill.mapIsaAttributes) {
    var attribute = attributes.find((attribute) => attribute.key === skill.mapIsaAttributes[index])
    increasedAttributes += attribute.increased ? 1 : 0
    sum += attribute.value
  }
  skill.value = Math.round((sum + increasedAttributes)/3)

  skill.increased = (skill.value == Math.round((sum)/3) ? false : true)
}

function getAttribute(key) {
  return attributes.find((attribute) => attribute.key === key)
}

function increase(attribute) {
  attribute.increased = !attribute.increased
  calcReferencedSkills(attribute.key)
}

function toggleFilter() {
  showOnlyIncreasedSkills.value = !showOnlyIncreasedSkills.value
}

</script>

<template>
<div class="container">
    <!-- Column for Attributes -->
    <div class="column">
      <h1>Eigenschaften</h1>
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
        <button :class="[attribute.increased ? 'highlight-button' : '']" @click="increase(attribute)">+</button>
      </div>
    </div>

    <!-- Column for Skill Groups -->
    <div class="column">
      <h1>Talente</h1>
      <button :class="[showOnlyIncreasedSkills ? 'highlight-button' : '']" @click="toggleFilter()">Nur erhöhte Talente</button>
      <div v-for="group in skillGroups" :key="group.name" class="skill-group">
        <h2>
          {{ group.name }}
          <span :class="[(group.increasedSkills > 0) ? 'highlight-badge' : 'badge']">{{ group.increasedSkills }}</span>
        </h2>
        <div v-for="skill in group.skills" :key="skill.key" class="skill-item">
          <div v-if="!showOnlyIncreasedSkills || (showOnlyIncreasedSkills && skill.increased)" :class="[skill.increased ? 'skill-info-highlighted' : 'skill-info']">
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

.skill-info-highlighted {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #3acf4b; /* Slightly darker greenish background on hover */
  color: #713604; /* Change the font color to white when active */
  border-radius: 8px; /* Rounded corners for a modern feel */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05); /* Soft shadow for depth */
  transition: background-color 0.3s ease, box-shadow 0.3s ease; /* Smooth transitions */
  font-weight: 600; /* Slightly bolder font for emphasis */
}

.skill-info-highlighted:hover {
  background-color: #51cf5f; /* Slightly darker blue on hover */
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1); /* More pronounced shadow on hover */
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
button.highlight-button {
  background-color: #3acf4b; /* Modern blue background for active state */
  color: #713604; /* Change the font color to white when active */
}

/* Hover state: Slight background change and shadow lift */
button:hover {
  background-color: #f5ebda; /* Light gray on hover for normal buttons */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15); /* Increase shadow for hover effect */
}

/* Hover state for active button */
button.highlight-button:hover {
  background-color: #51cf5f; /* Slightly darker blue on hover */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15); /* Increase shadow for hover effect */
}

/* Badge styling for the highlighted skill sum */
.badge {
  border: 1px solid #aaa;
  padding: 5px 10px;
  border-radius: 12px;
  font-size: 14px;
  margin-left: 10px;
}

/* Badge styling for the highlighted skill sum */
.highlight-badge {
  background-color: #3acf4b;
  color: #713604;
  padding: 5px 10px;
  border-radius: 12px;
  font-size: 14px;
  margin-left: 10px;
}

h3, h4 {
  margin-bottom: 15px;
}

</style>

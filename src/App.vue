<script setup lang="ts">
import { ref, reactive, computed } from 'vue'

const showOnlyIncreasedSkills = ref(false)
const showAssociatedAttributes = ref(false)
const showFormulas = ref(false)

const attributes = reactive([
  { key: 'MU', name: 'Mut', value: 8, increased: false },
  { key: 'KL', name: 'Klugheit', value: 8, increased: false },
  { key: 'IN', name: 'Intuition', value: 8, increased: false },
  { key: 'CH', name: 'Charisma', value: 8, increased: false },
  { key: 'FF', name: 'Fingerfertigkeit', value: 8, increased: false },
  { key: 'GE', name: 'Gewandheit', value: 8, increased: false },
  { key: 'KO', name: 'Konstitution', value: 8, increased: false },
  { key: 'KK', name: 'Körperkraft', value: 8, increased: false }
])

const skillGroups = reactive([
  {
    key: 'koerper',
    name: 'Körper',
    increasedSkills: 0,
    skills: [
      {
        key: 'akrobatik',
        name: 'Akrobatik',
        mapIsaAttributes: ['MU', 'GE', 'KK'],
        value: 8,
        increased: false,
        formula: '(MU + GE + KK) / 3'
      },
      {
        key: 'athletik',
        name: 'Athletik',
        mapIsaAttributes: ['GE', 'KO', 'KK'],
        value: 8,
        increased: false,
        formula: '(GE + KO + KK) / 3'
      },
      {
        key: 'diebeskunst',
        name: 'Diebeskunst',
        mapIsaAttributes: ['KL', 'IN', 'FF'],
        value: 8,
        increased: false,
        formula: '(KL + IN + FF) / 3'
      },
      {
        key: 'heimlichkeit',
        name: 'Heimlichkeit',
        mapIsaAttributes: ['MU', 'IN', 'GE'],
        value: 8,
        increased: false,
        formula: '(MU + IN + GE) / 3'
      },
      {
        key: 'horchen',
        name: 'Horchen',
        mapIsaAttributes: ['KL', 'IN', 'KO'],
        value: 8,
        increased: false,
        formula: '(KL + IN + KO) / 3'
      },
      {
        key: 'klettern',
        name: 'Klettern',
        mapIsaAttributes: ['MU', 'GE', 'KK'],
        value: 8,
        increased: false,
        formula: '(MU + GE + KK) / 3'
      },
      {
        key: 'reiten',
        name: 'Reiten',
        mapIsaAttributes: ['CH', 'GE', 'KK'],
        value: 8,
        increased: false,
        formula: '(CH + GE + KK) / 3'
      },
      {
        key: 'schwimmen',
        name: 'Schwimmen',
        mapIsaAttributes: ['GE', 'KO', 'KK'],
        value: 8,
        increased: false,
        formula: '(GE + KO + KK) / 3'
      },
      {
        key: 'verborgenesErkennen',
        name: 'Verborgenes Erkennen',
        mapIsaAttributes: ['MU', 'KL', 'IN'],
        value: 8,
        increased: false,
        formula: '(MU + KL + IN) / 3'
      },
      {
        key: 'zechen',
        name: 'Zechen',
        mapIsaAttributes: ['IN', 'KO', 'KK'],
        value: 8,
        increased: false,
        formula: '(IN + KO + KK) / 3'
      }
    ]
  },
  {
    key: 'natur',
    name: 'Natur',
    increasedSkills: 0,
    skills: [
      {
        key: 'faehrtensuche',
        name: 'Fährtensuche',
        mapIsaAttributes: ['KL', 'IN', 'KO'],
        value: 8,
        increased: false,
        formula: '(KL + IN + KO) / 3'
      },
      {
        key: 'fischenAngeln',
        name: 'Fischen & Angeln',
        mapIsaAttributes: ['IN', 'FF', 'KK'],
        value: 8,
        increased: false,
        formula: '(IN + FF + KK) / 3'
      },
      {
        key: 'himmelskunde',
        name: 'Himmelskunde',
        mapIsaAttributes: ['KL', 'IN', 'IN'],
        value: 8,
        increased: false,
        formula: '(KL + IN + IN) / 3'
      },
      {
        key: 'lagerRasten',
        name: 'Lager & Rasten',
        mapIsaAttributes: ['IN', 'GE', 'KO'],
        value: 8,
        increased: false,
        formula: '(IN + GE + KO) / 3'
      },
      {
        key: 'orientierung',
        name: 'Orientierung',
        mapIsaAttributes: ['KL', 'IN', 'IN'],
        value: 8,
        increased: false,
        formula: '(KL + IN + IN) / 3'
      },
      {
        key: 'pflanzenkunde',
        name: 'Pflanzenkunde',
        mapIsaAttributes: ['KL', 'IN', 'FF'],
        value: 8,
        increased: false,
        formula: '(KL + IN + FF) / 3'
      },
      {
        key: 'tierkunde',
        name: 'Tierkunde',
        mapIsaAttributes: ['MU', 'KL', 'IN'],
        value: 8,
        increased: false,
        formula: '(MU + IN + IN) / 3'
      }
    ]
  },
  {
    key: 'handwerk',
    name: 'Handwerk',
    increasedSkills: 0,
    skills: [
      {
        key: 'alchemie',
        name: 'Alchemie',
        mapIsaAttributes: ['MU', 'KL', 'FF'],
        value: 8,
        increased: false,
        formula: '(MU + KL + FF) / 3'
      },
      {
        key: 'bootSeefahrt',
        name: 'Boot- & Seefahrt',
        mapIsaAttributes: ['IN', 'GE', 'KO'],
        value: 8,
        increased: false,
        formula: '(IN + GE + KO) / 3'
      },
      {
        key: 'fahrzeugLenken',
        name: 'Fahrzeug lenken',
        mapIsaAttributes: ['IN', 'CH', 'FF'],
        value: 8,
        increased: false,
        formula: '(IN + CH + FF) / 3'
      },
      {
        key: 'holzbearbeitung',
        name: 'Holzbearbeitung',
        mapIsaAttributes: ['KL', 'FF', 'KK'],
        value: 8,
        increased: false,
        formula: '(KL + FF + KK) / 3'
      },
      {
        key: 'kochenBrauen',
        name: 'Kochen & Brauen',
        mapIsaAttributes: ['KL', 'FF', 'FF'],
        value: 8,
        increased: false,
        formula: '(KL + FF + FF) / 3'
      },
      {
        key: 'lehmSteinbearbeitung',
        name: 'Lehm- & Steinbearbeitung',
        mapIsaAttributes: ['KL', 'FF', 'FF'],
        value: 8,
        increased: false,
        formula: '(KL + FF + FF) / 3'
      },
      {
        key: 'malenZeichnen',
        name: 'Malen & Zeichnen',
        mapIsaAttributes: ['KL', 'IN', 'FF'],
        value: 8,
        increased: false,
        formula: '(KL + IN + FF) / 3'
      },
      {
        key: 'musizieren',
        name: 'Musizieren',
        mapIsaAttributes: ['IN', 'CH', 'FF'],
        value: 8,
        increased: false,
        formula: '(IN + CH + FF) / 3'
      },
      {
        key: 'schmiedekunst',
        name: 'Schmiedekunst',
        mapIsaAttributes: ['FF', 'KO', 'KK'],
        value: 8,
        increased: false,
        formula: '(FF + KO + KK) / 3'
      },
      {
        key: 'schneiderLederarbeiten',
        name: 'Schneider- & Lederarbeiten',
        mapIsaAttributes: ['KL', 'FF', 'KO'],
        value: 8,
        increased: false,
        formula: '(KL + FF + KO) / 3'
      }
    ]
  },
  {
    key: 'gesellschaft',
    name: 'Gesellschaft',
    increasedSkills: 0,
    skills: [
      {
        key: 'einschuechtern',
        name: 'Einschüchtern',
        mapIsaAttributes: ['MU', 'CH', 'KK'],
        value: 8,
        increased: false,
        formula: '(MU + CH + KK) / 3'
      },
      {
        key: 'handel',
        name: 'Handel',
        mapIsaAttributes: ['KL', 'IN', 'CH'],
        value: 8,
        increased: false,
        formula: '(KL + IN + CH) / 3'
      },
      {
        key: 'schauspielerei',
        name: 'Schauspielerei',
        mapIsaAttributes: ['MU', 'KL', 'CH'],
        value: 8,
        increased: false,
        formula: '(MU + KL + CH) / 3'
      },
      {
        key: 'standeswissen',
        name: 'Standeswissen',
        mapIsaAttributes: ['KL', 'IN', 'CH'],
        value: 8,
        increased: false,
        formula: '(KL + IN + CH) / 3'
      },
      {
        key: 'tanzen',
        name: 'Tanzen',
        mapIsaAttributes: ['CH', 'GE', 'GE'],
        value: 8,
        increased: false,
        formula: '(CH + GE + GE) / 3'
      },
      {
        key: 'ueberreden',
        name: 'Überreden',
        mapIsaAttributes: ['MU', 'IN', 'CH'],
        value: 8,
        increased: false,
        formula: '(MU + IN + CH) / 3'
      },
      {
        key: 'ueberzeugen',
        name: 'Überzeugen',
        mapIsaAttributes: ['KL', 'IN', 'CH'],
        value: 8,
        increased: false,
        formula: '(KL + IN + CH) / 3'
      }
    ]
  },
  {
    key: 'wissen',
    name: 'Wissen',
    increasedSkills: 0,
    skills: [
      {
        key: 'architekt',
        name: 'Architekt',
        mapIsaAttributes: ['KL', 'KL', 'FF'],
        value: 8,
        increased: false,
        formula: '(KL + KL + FF) / 3'
      },
      {
        key: 'geographie',
        name: 'Geographie',
        mapIsaAttributes: ['KL', 'KL', 'IN'],
        value: 8,
        increased: false,
        formula: '(KL + KL + IN) / 3'
      },
      {
        key: 'geschichtswissen',
        name: 'Geschichtswissen',
        mapIsaAttributes: ['KL', 'KL', 'IN'],
        value: 8,
        increased: false,
        formula: '(KL + KL + IN) / 3'
      },
      {
        key: 'goetterKulte',
        name: 'Götter & Kulte',
        mapIsaAttributes: ['KL', 'KL', 'IN'],
        value: 8,
        increased: false,
        formula: '(KL + KL + IN) / 3'
      },
      {
        key: 'heilkundeGeist',
        name: '(Heil-)Kunde des Geistes',
        mapIsaAttributes: ['KL', 'IN', 'CH'],
        value: 8,
        increased: false,
        formula: '(KL + IN + CH) / 3'
      },
      {
        key: 'heilkundeKoerper',
        name: '(Heil-)Kunde des Körpers',
        mapIsaAttributes: ['KL', 'IN', 'FF'],
        value: 8,
        increased: false,
        formula: '(KL + IN + FF) / 3'
      },
      {
        key: 'heilkundeSaefte',
        name: '(Heil-)Kunde der Säfte',
        mapIsaAttributes: ['KL', 'KL', 'IN'],
        value: 8,
        increased: false,
        formula: '(KL + KL + IN) / 3'
      },
      {
        key: 'kriegskunst',
        name: 'Kriegskunst',
        mapIsaAttributes: ['MU', 'KL', 'CH'],
        value: 8,
        increased: false,
        formula: '(MU + KL + CH) / 3'
      },
      {
        key: 'magiekunde',
        name: 'Magiekunde',
        mapIsaAttributes: ['KL', 'KL', 'IN'],
        value: 8,
        increased: false,
        formula: '(KL + KL + IN) / 3'
      },
      {
        key: 'rechnenPhysik',
        name: 'Rechnen & Physik',
        mapIsaAttributes: ['KL', 'IN', 'FF'],
        value: 8,
        increased: false,
        formula: '(KL + IN + FF) / 3'
      },
      {
        key: 'rechtsStaatskunst',
        name: 'Rechts- & Staatskunst',
        mapIsaAttributes: ['KL', 'KL', 'IN'],
        value: 8,
        increased: false,
        formula: '(KL + KL + IN) / 3'
      }
    ]
  }
])

const combatGroup = reactive({
  key: 'kampf',
  name: 'Kampf',
  increasedSkills: 0,
  skills: [
    {
      key: 'dolchFechtwaffen',
      name: 'Dolch- & Fechtwaffen',
      mapIsaAttributes: ['MU', 'IN', 'FF', 'GE'],
      highestAt: {
        value: 8,
        mapIsaAttributes: ['MU', 'FF'],
        increased: false,
        formula: '(MU + FF) / 2'
      },
      highestPa: {
        value: 8,
        mapIsaAttributes: ['IN', 'FF'],
        increased: false,
        formula: '(IN + FF) / 2'
      },
      attackCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['MU', 'FF'],
          increased: false,
          formula: '(MU + FF) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['MU', 'GE'],
          increased: false,
          formula: '(MU + GE) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['FF', 'GE'],
          increased: false,
          formula: '(FF + GE) / 2'
        }
      ],
      paCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['IN', 'FF'],
          increased: false,
          formula: '(IN + FF) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['IN', 'GE'],
          increased: false,
          formula: '(IN + GE) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['FF', 'GE'],
          increased: false,
          formula: '(FF + GE) / 2'
        }
      ],
      increased: false
    },
    {
      key: 'hiebKettenwaffen',
      name: 'Hieb- & Kettenwaffen',
      mapIsaAttributes: ['MU', 'IN', 'KO', 'KK'],
      highestAt: {
        value: 8,
        mapIsaAttributes: ['MU', 'KO'],
        increased: false,
        formula: '(MU + KO) / 2'
      },
      highestPa: {
        value: 8,
        mapIsaAttributes: ['IN', 'KO'],
        increased: false,
        formula: '(IN + KO) / 2'
      },
      attackCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['MU', 'KO'],
          increased: false,
          formula: '(MU + KO) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['MU', 'KK'],
          increased: false,
          formula: '(MU + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['KO', 'KK'],
          increased: false,
          formula: '(KO + KK) / 2'
        }
      ],
      paCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['IN', 'KO'],
          increased: false,
          formula: '(IN + KO) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['IN', 'KK'],
          increased: false,
          formula: '(IN + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['KO', 'KK'],
          increased: false,
          formula: '(KO + KK) / 2'
        }
      ],
      increased: false
    },
    {
      key: 'raufenRingen',
      name: 'Raufen & Ringen',
      mapIsaAttributes: ['MU', 'IN', 'GE', 'KK'],
      highestAt: {
        value: 8,
        mapIsaAttributes: ['MU', 'GE'],
        increased: false,
        formula: '(MU + GE) / 2'
      },
      highestPa: {
        value: 8,
        mapIsaAttributes: ['IN', 'GE'],
        increased: false,
        formula: '(IN + GE) / 2'
      },
      attackCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['MU', 'GE'],
          increased: false,
          formula: '(MU + GE) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['MU', 'KK'],
          increased: false,
          formula: '(MU + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['GE', 'KK'],
          increased: false,
          formula: '(GE + KK) / 2'
        }
      ],
      paCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['IN', 'GE'],
          increased: false,
          formula: '(IN + GE) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['IN', 'KK'],
          increased: false,
          formula: '(IN + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['GE', 'KK'],
          increased: false,
          formula: '(GE + KK) / 2'
        }
      ],
      increased: false
    },
    {
      key: 'saebelSchwerter',
      name: 'Säbel & Schwerter',
      mapIsaAttributes: ['MU', 'IN', 'GE', 'KK'],
      highestAt: {
        value: 8,
        mapIsaAttributes: ['MU', 'GE'],
        increased: false,
        formula: '(MU + GE) / 2'
      },
      highestPa: {
        value: 8,
        mapIsaAttributes: ['IN', 'GE'],
        increased: false,
        formula: '(IN + GE) / 2'
      },
      attackCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['MU', 'GE'],
          increased: false,
          formula: '(MU + GE) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['MU', 'KK'],
          increased: false,
          formula: '(MU + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['GE', 'KK'],
          increased: false,
          formula: '(GE + KK) / 2'
        }
      ],
      paCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['IN', 'GE'],
          increased: false,
          formula: '(IN + GE) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['IN', 'KK'],
          increased: false,
          formula: '(IN + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['GE', 'KK'],
          increased: false,
          formula: '(GE + KK) / 2'
        }
      ],
      increased: false
    },
    {
      key: 'speerStabwaffen',
      name: 'Speer & Stabwaffen',
      mapIsaAttributes: ['MU', 'IN', 'FF', 'KK'],
      highestAt: {
        value: 8,
        mapIsaAttributes: ['MU', 'FF'],
        increased: false,
        formula: '(MU + FF) / 2'
      },
      highestPa: {
        value: 8,
        mapIsaAttributes: ['IN', 'FF'],
        increased: false,
        formula: '(IN + FF) / 2'
      },
      attackCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['MU', 'FF'],
          increased: false,
          formula: '(MU + FF) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['MU', 'KK'],
          increased: false,
          formula: '(MU + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['FF', 'KK'],
          increased: false,
          formula: '(FF + KK) / 2'
        }
      ],
      paCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['IN', 'FF'],
          increased: false,
          formula: '(IN + FF) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['IN', 'KK'],
          increased: false,
          formula: '(IN + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['FF', 'KK'],
          increased: false,
          formula: '(FF + KK) / 2'
        }
      ],
      increased: false
    },
    {
      key: 'armbrust',
      name: 'Armbrust',
      mapIsaAttributes: ['IN', 'FF', 'KK'],
      highestAt: {
        value: 8,
        mapIsaAttributes: ['IN', 'FF'],
        increased: false,
        formula: '(FF + KK) / 2'
      },
      highestPa: {
        value: 8,
        mapIsaAttributes: ['IN', 'FF'],
        increased: false,
        formula: '(FF + KK) / 2'
      },
      attackCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['IN', 'FF'],
          increased: false,
          formula: '(IN + FF) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['IN', 'KK'],
          increased: false,
          formula: '(IN + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['FF', 'KK'],
          increased: false,
          formula: '(FF + KK) / 2'
        }
      ],
      paCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['IN', 'FF'],
          increased: false,
          formula: '(IN + FF) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['IN', 'KK'],
          increased: false,
          formula: '(IN + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['FF', 'KK'],
          increased: false,
          formula: '(FF + KK) / 2'
        }
      ],
      increased: false
    },
    {
      key: 'bogen',
      name: 'Bogen',
      mapIsaAttributes: ['FF', 'GE', 'KK'],
      highestAt: {
        value: 8,
        mapIsaAttributes: ['FF', 'GE'],
        increased: false,
        formula: '(FF + GE) / 2'
      },
      highestPa: {
        value: 8,
        mapIsaAttributes: ['FF', 'GE'],
        increased: false,
        formula: '(FF + GE) / 2'
      },
      attackCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['FF', 'GE'],
          increased: false,
          formula: '(FF + GE) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['FF', 'KK'],
          increased: false,
          formula: '(FF + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['GE', 'KK'],
          increased: false,
          formula: '(GE + KK) / 2'
        }
      ],
      paCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['FF', 'GE'],
          increased: false,
          formula: '(FF + GE) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['FF', 'KK'],
          increased: false,
          formula: '(FF + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['GE', 'KK'],
          increased: false,
          formula: '(GE + KK) / 2'
        }
      ],
      increased: false
    },
    {
      key: 'torsionswaffen',
      name: 'Torsionswaffen',
      mapIsaAttributes: ['KL', 'IN', 'FF'],
      highestAt: {
        value: 8,
        mapIsaAttributes: ['KL', 'IN'],
        increased: false,
        formula: '(KL + IN) / 2'
      },
      highestPa: {
        value: 8,
        mapIsaAttributes: ['KL', 'IN'],
        increased: false,
        formula: '(KL + IN) / 2'
      },
      attackCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['KL', 'IN'],
          increased: false,
          formula: '(KL + IN) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['KL', 'FF'],
          increased: false,
          formula: '(KL + FF) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['IN', 'FF'],
          increased: false,
          formula: '(IN + FF) / 2'
        }
      ],
      paCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['KL', 'IN'],
          increased: false,
          formula: '(KL + IN) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['KL', 'FF'],
          increased: false,
          formula: '(KL + FF) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['IN', 'FF'],
          increased: false,
          formula: '(IN + FF) / 2'
        }
      ],
      increased: false
    },
    {
      key: 'wurfSchleuderwaffen',
      name: 'Wurf- & Schleuderwaffen',
      mapIsaAttributes: ['FF', 'GE', 'KK'],
      highestAt: {
        value: 8,
        mapIsaAttributes: ['FF', 'GE'],
        increased: false,
        formula: '(FF + GE) / 2'
      },
      highestPa: {
        value: 8,
        mapIsaAttributes: ['FF', 'GE'],
        increased: false,
        formula: '(FF + GE) / 2'
      },
      attackCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['FF', 'GE'],
          increased: false,
          formula: '(FF + GE) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['FF', 'KK'],
          increased: false,
          formula: '(FF + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['GE', 'KK'],
          increased: false,
          formula: '(GE + KK) / 2'
        }
      ],
      paCombinations: [
        {
          value: 8,
          mapIsaAttributes: ['FF', 'GE'],
          increased: false,
          formula: '(FF + GE) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['FF', 'KK'],
          increased: false,
          formula: '(FF + KK) / 2'
        },
        {
          value: 8,
          mapIsaAttributes: ['GE', 'KK'],
          increased: false,
          formula: '(GE + KK) / 2'
        }
      ],
      increased: false
    }
  ]
})

const combinedAttributes = reactive({
  key: 'combinedAttributes',
  name: 'Abgeleitete Eigenschaften',
  increasedSkills: 0,
  skills: [
    {
      key: 'robustheit',
      name: 'Robustheit',
      mapIsaAttributes: ['KO', 'KO', 'KK'],
      value: 8,
      divider: 3,
      increased: false,
      formula: '(KO + KO + KO) / 3'
    },
    {
      key: 'mumm',
      name: 'Mumm',
      mapIsaAttributes: ['MU', 'MU', 'KL'],
      value: 5,
      divider: 5,
      increased: false,
      formula: '(MU + MU + KL) / 5'
    },
    {
      key: 'bewegung',
      name: 'Bewegung',
      mapIsaAttributes: ['GE', 'GE', 'IN'],
      value: 5,
      divider: 5,
      increased: false,
      formula: '(GE + GE + IN) / 5'
    },
    {
      key: 'reflexe',
      name: 'Reflexe',
      mapIsaAttributes: ['KL', 'IN', 'GE'],
      value: 8,
      divider: 3,
      increased: false,
      formula: '(KL + IN + GE) / 3'
    },
    {
      key: 'sprachbegabung',
      name: 'Sprachbegabung',
      mapIsaAttributes: ['KL', 'IN', 'CH'],
      divider: 3,
      value: 8,
      increased: false,
      formula: '(KL + IN + CH) / 3'
    }
  ]
})

function calcEverything(attribute) {
  // problem: last attribute to calc roudn wins if attribute is set or not
  attributes.forEach((attriubte) => {
    calcAssociatedSkills(attriubte.key)
    calcReferencedCombinedAttributes(attriubte.key)
    calcAssociatedCombatSkills(attriubte.key)
  })
}

function calcEverythingAssociated(key) {
  calcAssociatedSkills(key)
  calcReferencedCombinedAttributes(key)
  calcAssociatedCombatSkills(key)
}

function calcReferencedCombinedAttributes(attributeKey: string) {
  combinedAttributes.skills
    .filter((skill) => skill.mapIsaAttributes.includes(attributeKey))
    .forEach((skill) => {
      calcCombinedAttribute(skill)
    })
  combinedAttributes.increasedSkills = combinedAttributes.skills.filter(
    (skill) => skill.increased === true
  ).length
}

function calcAssociatedCombatSkills(attributeKey: string) {
  combatGroup.skills
    .filter((skill) => skill.mapIsaAttributes.includes(attributeKey))
    .forEach((skill) => {
      // alert('reached skill: ' + skill.name)
      calcCombatSkill(skill, attributeKey)
    })
  combatGroup.increasedSkills = combatGroup.skills.filter(
    (skill) => skill.increased === true
  ).length
}

function calcAssociatedSkills(attributeKey: string) {
  skillGroups.forEach((group) => {
    group.skills
      .filter((skill) => skill.mapIsaAttributes.includes(attributeKey))
      .forEach((skill) => {
        calcSkill(skill)
      })
    group.increasedSkills = group.skills.filter((skill) => skill.increased === true).length
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
  skill.value = Math.round(sum / 3)
  skill.increased = Math.round(increasedAttributes / 3) == 0 ? false : true
}

function calcCombinedAttribute(combAttr) {
  var increasedAttributes = 0
  var sum = 0
  for (var index in combAttr.mapIsaAttributes) {
    var attribute = attributes.find(
      (attribute) => attribute.key === combAttr.mapIsaAttributes[index]
    )
    increasedAttributes += attribute.increased ? 1 : 0
    sum += attribute.value
  }
  combAttr.value = Math.round(sum / combAttr.divider)
  // combinedAttributes needs a more complicated check due to possible higher divider values
  combAttr.increased =
    combAttr.value == Math.round((sum - increasedAttributes) / combAttr.divider) ? false : true
}

function calcCombatSkill(skill, attributeKey) {
  calculateHighestAttackOrBlock(skill.attackCombinations, attributeKey)
  calculateHighestAttackOrBlock(skill.paCombinations, attributeKey)
  skill.attackCombinations.forEach((combination) => {
    skill.highestAt = skill.highestAt.value < combination.value ? combination : skill.highestAt
  })
  skill.paCombinations.forEach((combination) => {
    skill.highestPa = skill.highestPa.value < combination.value ? combination : skill.highestPa
  })

  // check if skill increased
  skill.increased = skill.highestAt.increased || skill.highestPa.increased ? true : false
}

function calculateHighestAttackOrBlock(combinations, attributeKey) {
  var sum
  var increasedAttributes
  combinations
    .filter((combination) => combination.mapIsaAttributes.includes(attributeKey))
    .forEach((combination) => {
      sum = 0
      increasedAttributes = 0
      for (var index in combination.mapIsaAttributes) {
        var attribute = attributes.find(
          (attribute) => attribute.key === combination.mapIsaAttributes[index]
        )
        increasedAttributes += attribute.increased ? 1 : 0
        sum += attribute.value
      }
      combination.value = Math.round(sum / 2)
      combination.increased = increasedAttributes > 0 ? true : false
    })
}

function increaseAttribute(attribute) {
  attribute.increased = !attribute.increased
  attribute.value = attribute.increased ? attribute.value + 1 : attribute.value - 1
  // calcEverythingAssociated(attribute.key)
  calcEverythingAssociated(attribute.key)
}

function setAttribute(attribute) {
  attribute.increased = false
  calcEverythingAssociated(attribute.key)
  calcEverythingAssociated(attribute.key)
}

function toggleShowOnlyIncreasedSkills() {
  showOnlyIncreasedSkills.value = !showOnlyIncreasedSkills.value
}

function toggleShowAssociatedAttributes() {
  showAssociatedAttributes.value = !showAssociatedAttributes.value
}

function toggleShowFormulas() {
  showFormulas.value = !showFormulas.value
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
        </div>
        <input
          type="number"
          min="8"
          max="16"
          @change="setAttribute(attribute)"
          v-model.number="attribute.value"
          :class="[attribute.increased ? 'attribute-input-highlighted' : 'attribute-input']"
        />
        <button
          :class="[attribute.increased ? 'highlight-button' : '']"
          @click="increaseAttribute(attribute)"
        >
          +
        </button>
      </div>
    </div>

    <!-- Column for calculated values -->
    <div class="column">
      <h1>Talente</h1>
      <button
        :class="[showOnlyIncreasedSkills ? 'highlight-button' : '']"
        @click="toggleShowOnlyIncreasedSkills()"
      >
        Nur erhöhte Talente
      </button>

      <button
        :class="[showAssociatedAttributes ? 'highlight-button' : '']"
        @click="toggleShowAssociatedAttributes()"
      >
        Talentattribute anzeigen
      </button>

      <button :class="[showFormulas ? 'highlight-button' : '']" @click="toggleShowFormulas()">
        Formeln anzeigen
      </button>

      <!-- combined attributes -->
      <h2>
        {{ combinedAttributes.name }}
        <span :class="[combinedAttributes.increasedSkills > 0 ? 'highlight-badge' : 'badge']">{{
          combinedAttributes.increasedSkills
        }}</span>
      </h2>
      <div v-for="skill in combinedAttributes.skills" :key="skill.key" class="skill-item">
        <div
          v-if="!showOnlyIncreasedSkills || (showOnlyIncreasedSkills && skill.increased)"
          :class="[skill.increased ? 'skill-info-highlighted' : 'skill-info']"
        >
          <span class="skill-name">{{ skill.name }}</span>
          <span class="skill-value">{{ skill.value }}</span>
          <span v-if="showAssociatedAttributes" class="skill-attributes">{{
            skill.mapIsaAttributes
          }}</span>
          <span v-if="showFormulas" class="skill-attributes">{{ skill.formula }}</span>
        </div>
      </div>

      <!-- combat skills -->
      <h2>
        {{ combatGroup.name }}
        <span :class="[combatGroup.increasedSkills > 0 ? 'highlight-badge' : 'badge']">{{
          combatGroup.increasedSkills
        }}</span>
      </h2>
      <div v-for="skill in combatGroup.skills" :key="skill.key" class="skill-item">
        <div
          v-if="!showOnlyIncreasedSkills || (showOnlyIncreasedSkills && skill.increased)"
          class="skill-info"
        >
          <span
            class="skill-name"
            :class="[skill.increased ? 'combatSkill-info-highlighted' : 'combatSkill-info']"
            >{{ skill.name }}</span
          >
          <span
            class="skill-value"
            :class="[skill.highestAt?.increased ? 'attackOrBlock-highlighted' : 'attackOrBlock']"
            >AT: {{ skill.highestAt?.value }}</span
          >
          <span
            class="skill-value"
            :class="[skill.highestPa?.increased ? 'attackOrBlock-highlighted' : 'attackOrBlock']"
            >PA: {{ skill.highestPa?.value }}</span
          >
          <span
            class="skill-attributes"
            v-if="showAssociatedAttributes"
            :title="skill.mapIsaAttributes.toString()"
            >{{ skill.mapIsaAttributes }}</span
          >
          <span class="skill-attributes" v-if="showFormulas"
            >AT Formel: {{ skill.highestAt.formula }}</span
          >
          <span class="skill-attributes" v-if="showFormulas"
            >PA Formel: {{ skill.highestPa.formula }}</span
          >
        </div>
      </div>

      <!-- all other skills -->
      <div v-for="group in skillGroups" :key="group.name" class="skill-group">
        <h2>
          {{ group.name }}
          <span :class="[group.increasedSkills > 0 ? 'highlight-badge' : 'badge']">{{
            group.increasedSkills
          }}</span>
        </h2>
        <div v-for="skill in group.skills" :key="skill.key" class="skill-item">
          <div
            v-if="!showOnlyIncreasedSkills || (showOnlyIncreasedSkills && skill.increased)"
            :class="[skill.increased ? 'skill-info-highlighted' : 'skill-info']"
          >
            <span class="skill-name">{{ skill.name }}</span>
            <span class="skill-value">{{ skill.value }}</span>
            <span v-if="showAssociatedAttributes" class="skill-attributes">{{
              skill.mapIsaAttributes
            }}</span>
            <span v-if="showFormulas" class="skill-attributes">{{ skill.formula }}</span>
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

.attribute-item,
.skill-item {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  white-space: nowrap; /* Prevent line breaks within the items */
}

.attribute-info,
.skill-info {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.combatSkill-info {
  display: flex;
  justify-content: space-between;
  width: 60%;
}

.attackOrBlock {
  display: flex;
  justify-content: space-between;
  width: 20%;
}

.skill-info-highlighted {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #3acf4b; /* Slightly darker greenish background on hover */
  color: #713604; /* Change the font color to white when active */
  border-radius: 8px; /* Rounded corners for a modern feel */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05); /* Soft shadow for depth */
  transition:
    background-color 0.3s ease,
    box-shadow 0.3s ease; /* Smooth transitions */
  font-weight: bold; /* bold font for emphasis */
  width: 100%;
}

.combatSkill-info-highlighted {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #3acf4b; /* Slightly darker greenish background on hover */
  color: #713604; /* Change the font color to white when active */
  border-radius: 8px; /* Rounded corners for a modern feel */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05); /* Soft shadow for depth */
  transition:
    background-color 0.3s ease,
    box-shadow 0.3s ease; /* Smooth transitions */
  font-weight: bold; /* bold font for emphasis */
  width: 60%;
}

.attackOrBlock-highlighted {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #3acf4b; /* Slightly darker greenish background on hover */
  color: #713604; /* Change the font color to white when active */
  border-radius: 8px; /* Rounded corners for a modern feel */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05); /* Soft shadow for depth */
  transition:
    background-color 0.3s ease,
    box-shadow 0.3s ease; /* Smooth transitions */
  font-weight: bold; /* bold font for emphasis */
  width: 20%;
}

.skill-info-highlighted:hover,
.combatSkill-info-highlighted:hover,
.attackOrBlock-highlighted:hover {
  background-color: #51cf5f; /* Slightly darker blue on hover */
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1); /* More pronounced shadow on hover */
}

.attribute-name,
.attribute-key,
.attribute-value,
.skill-key,
.skill-name,
.skill-attributes,
.skill-value {
  margin-right: 10px;
  flex-shrink: 0; /* Prevent shrinking */
  overflow: hidden;
  text-overflow: ellipsis; /* Truncate text with ellipsis if it overflows */
}

.attribute-input {
  flex-shrink: 0;
}

.attribute-input-highlighted {
  flex-shrink: 0;
  color: #3acf4b;
}

button {
  padding: 10px 20px;
  margin: 10px;
  border: none;
  border-radius: 8px; /* Softer edges with rounded corners */
  background-color: #f8f2e8; /* Light gray background */
  color: #333; /* Darker font color */
  font-size: 16px;
  cursor: pointer;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* Soft shadow for depth */
  transition:
    background-color 0.3s ease,
    box-shadow 0.3s ease; /* Smooth transition effects */
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

h3,
h4 {
  margin-bottom: 15px;
}

input {
  background-color: #2c3e50; /* Darker background for modern look */
  color: #ecf0f1; /* Light text for contrast */
  border: 1px solid #34495e; /* Subtle, softer border */
  padding: 10px 15px;
  border-radius: 6px; /* Rounded corners */
  font-size: 16px;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1); /* Subtle inner shadow for depth */
  transition:
    border-color 0.3s ease,
    box-shadow 0.3s ease; /* Smooth transitions */
}

input:focus {
  border-color: #1abc9c; /* Slightly brighter border on focus */
  box-shadow: 0 0 5px rgba(26, 188, 156, 0.5); /* Add a subtle glow effect on focus */
  outline: none; /* Remove the default outline */
}

input::placeholder {
  color: #7f8c8d; /* Slightly lighter placeholder text */
}
</style>

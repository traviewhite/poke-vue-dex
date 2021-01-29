<template>
  <div v-if="types" class="types">
    {{ types.type }}
  </div>
</template>

<script>
import getMultipliers from './getMultipliers.js'
export default {
  name: 'Multiply',
  props: ['currentUrl'],
  data: () => ({
    types: [],
    type: {},
  }),
  methods: {
    effective() {
      if (currentUrl && currentUrl.types) {
        let types = currentUrl.types.map((type) => {
          return type.name
        })
        const multipliers = getMultipliers(types)
        let multipliersAttack = new Set()
        let multipliersDefense = new Set()

        Object.keys(multipliers.attack).forEach((key) => {
          multipliersAttack.add(multipliers.attack[key])
        })
        Object.keys(multipliers.defense).forEach((key) => {
          multipliersDefense.add(multipliers.defense[key])
        })

        for (const [key, value] of Object.entries(multipliers.attack)) {
          if (value === 1) {
            delete multipliers.attack[key]
          }
        }

        for (const [key, value] of Object.entries(multipliers.defense)) {
          if (value === 1) {
            delete multipliers.defense[key]
          }
        }
      }
    },
  },
}
</script>

<style lang="scss">
@import '../styles/variables.scss';
</style>

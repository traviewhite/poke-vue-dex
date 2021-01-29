<template>
  <div class="search-bar">
    <input type="text" placeholder="Search For Pokémon 🤠 " v-model="search" @keydown.enter="findName" />
  </div>
</template>

<script>
export default {
  props: ['apiUrl'],
  data: () => {
    return {
      search: '',
    }
  },
  methods: {
    findName() {
      if (this.search !== '') this.$emit('setPokemonUrl', this.apiUrl + this.search)
    },
  },
}
</script>

<style lang="scss">
@import '../styles/variables.scss';

.search-bar {
  flex-basis: 40%;
  max-width: 420px;
  width: 95%;
  position: relative;
  @media screen and (max-width: $tablet) {
    margin: 1rem 0;
  }
  input {
    width: 100%;
    padding: 0.75rem;
    font-size: 1rem;
    border-radius: 5px;
    border: none;
    outline: none;
    box-shadow: 1px 2px 8px rgba(0, 0, 0, 0.1);
  }
}
</style>

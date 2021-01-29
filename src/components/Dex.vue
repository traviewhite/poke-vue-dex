<template>
  <main>
    <header>
      <div class="title">
        <h1>Poké-Vue-Dex</h1>
        <p>pokédex built for finding type weaknesses</p>
        <a href="https://traviewhite.com" target="_blank" rel="noopener noreferrer">
          <h5>traviewhite.com</h5>
        </a>
      </div>
      <Search :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl" />
    </header>
    <List :spriteUrl="spriteUrl" :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl" />
    <Modal v-if="showModal" :pokemonUrl="pokemonUrl" :imageUrl="imageUrl" @closeModal="closeModal" />
  </main>
</template>

<script>
import Search from './Search.vue'
import List from './List.vue'
import Modal from './Modal.vue'
export default {
  name: 'Dex',
  data() {
    return {
      spriteUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
      imageUrl: 'https://pokeres.bastionbot.org/images/pokemon/',
      apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
      pokemonUrl: '',
      showModal: false,
    }
  },
  components: {
    Search,
    List,
    Modal,
  },
  methods: {
    setPokemonUrl(url) {
      this.pokemonUrl = url
      this.showModal = true
    },
    closeModal() {
      this.pokemonUrl = ''
      this.showModal = false
    },
  },
}
</script>

<style lang="scss">
@import '../styles/variables.scss';
main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 1rem 1rem;
  header {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    max-width: 900px;
    width: 100%;
    margin-bottom: 2rem;
    @media screen and (max-width: $tablet) {
      flex-direction: column;
    }
    .title {
      @media screen and (max-width: $tablet) {
        text-align: center;
        margin: 2rem auto;
      }
      h1 {
        font-size: 2.5rem;
        color: $header-color;
        font-weight: 800;
        cursor: default;
        @media screen and (max-width: $tablet) {
          font-size: 2.75rem;
        }
      }
      p {
        font-size: 0.8rem;
        color: $faded;
        cursor: default;
      }
      a {
        h5 {
          display: inline-block;
          margin-top: 0.75rem;
          padding: 0.25rem 1rem;
          font-size: 0.8rem;
          letter-spacing: 0.5px;
          color: $header-color;
          background-color: $blue;
          border-radius: 5px;
          transition: 0.2s ease;
          &:hover {
            text-decoration: underline;
            color: $header-color;
            transform: translateY(-3px) translateX(-3px);
            box-shadow: 3px 3px 0 $faded;
          }
          @media screen and (max-width: $tablet) {
            font-size: 0.8rem;
          }
        }
      }
    }
  }
}
</style>

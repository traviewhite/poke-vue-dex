<template>
  <ul>
    <li v-for="(pokemon, i) in pokemons" :key="i" @click="setPokemonUrl(pokemon.url)">
      <img :src="spriteUrl + pokemon.id + '.png'" height="96" width="96" :alt="pokemon.name" />
      <h3>{{ pokemon.name }}</h3>
      <h5>#{{ pokemon.id }}</h5>
    </li>
  </ul>
  <div id="scroll-trigger" ref="infinitescrolltrigger" @click="next()">
    <p>LOAD MORE</p>
  </div>
</template>

<script>
export default {
  name: 'List',
  props: ['spriteUrl', 'apiUrl', 'imageUrl'],
  data: () => ({
    pokemons: [],
    nextUrl: '',
    currentUrl: '',
  }),
  methods: {
    async find() {
      const req = new Request(this.currentUrl)
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json()
        })
        .then((data) => {
          this.nextUrl = data.next
          data.results.forEach((pokemon) => {
            pokemon.id = pokemon.url
              .split('/')
              .filter(function (piece) {
                return !!piece
              })
              .pop()
            this.pokemons.push(pokemon)
          })
        })
        .catch((error) => {
          console.log(error)
        })
    },
    scrollTrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.next()
          }
        })
      })
      observer.observe(this.$refs.infinitescrolltrigger)
    },
    next() {
      this.currentUrl = this.nextUrl
      this.find()
    },
    setPokemonUrl(url) {
      this.$emit('setPokemonUrl', url)
    },
  },
  created() {
    this.currentUrl = this.apiUrl
    this.find()
  },
  mounted() {
    this.scrollTrigger()
  },
}
</script>

<style lang="scss">
@import '../styles/variables.scss';

ul {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-between;
  max-width: 900px;
  width: 100%;
  @media screen and (max-width: $tablet) {
    justify-content: space-evenly;
  }
  li {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    /* padding: 0.25rem; */
    margin: 0.5rem;
    height: 155px;
    width: 155px;
    text-align: center;
    text-transform: capitalize;
    border-radius: 50%;
    background-color: $header-color;
    box-shadow: 1px 2px 8px rgba(0, 0, 0, 0.1);
    position: relative;
    list-style: none;
    cursor: pointer;
    transition: 0.2s ease;
    &:hover {
      transform: translateY(-3px) translateX(-3px);
      box-shadow: 3px 3px 0 $faded;
    }
    h3 {
      margin-top: -0.5rem;
      font-size: 1rem;
      color: $red;
    }
    h4 {
      font-size: 1rem;
    }
    h5 {
      font-size: 1rem;
      position: absolute;
      top: 0;
      left: 0;
      width: 3rem;
      height: 3rem;
      display: flex;
      justify-content: center;
      align-items: center;
      /* margin: 5%; */
      color: $header-color;
      background-color: $blue;
      border-radius: 50%;
      box-shadow: 1px 2px 6px rgba(0, 0, 0, 0.1);
    }
  }
}
#scroll-trigger {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  p {
    margin: 2rem 0;
    padding: 0.5rem 1.5rem;
    border-radius: 5px;
    background-color: $blue;
    color: $header-color;
    font-size: 1.5rem;
    font-weight: 600;
    letter-spacing: 0.25px;
    cursor: pointer;
    &:hover {
      text-decoration: underline;
      color: $header-color;
    }
  }
}
</style>

<template>
  <div class="modal" @keydown.esc="closeModal" tabindex="0">
    <div class="modal-view" v-if="show">
      <div v-if="pokemon" class="image">
        <img :src="imageUrl + pokemon.id + '.png'" :alt="pokemon.name" />
        <div class="floor" />
      </div>
      <div v-if="pokemon" class="info">
        <div class="head-title">
          <h2>{{ pokemon.name }}</h2>
        </div>
        <div class="head-info">
          <div class="abilities">
            <div class="ability" v-for="(a, i) in pokemon.abilities" :key="i">
              <h5>{{ a.ability.name }}</h5>
            </div>
          </div>
          <div class="properties">
            <div class="property">
              <p>Height:</p>
              <p>{{ pokemon.height / 10 }} m</p>
            </div>
            <div class="property">
              <p>Weight:</p>
              <p>{{ pokemon.weight / 10 }} kg</p>
            </div>
          </div>
        </div>
        <h3>Types</h3>
        <div class="types">
          <div class="type" v-for="(t, i) in pokemon.types" :key="i">
            {{ t.type.name }}
          </div>
        </div>
        <!-- <Multiply :currentUrl="currentUrl" /> -->
        <!-- <div id="next">
          <button @click="nextPokemon">👉</button>
        </div>
        <div id="prev">
          <button @click="prevPokemon">👈</button>
        </div> -->
      </div>
      <div class="sorry" v-else>
        <h2>that pokémon does not exist 😢</h2>
        <p @click="closeModal">PLEASE TRY AGAIN</p>
      </div>
      <button class="x" @click="closeModal">✕</button>
      <div class="background" />
    </div>
  </div>
</template>

<script>
import Multiply from './Multiply.vue'
export default {
  components: { Multiply },
  name: 'Modal',
  props: ['imageUrl', 'pokemonUrl'],
  data: () => ({
    show: false,
    pokemons: [],
    pokemon: {},
    currentUrl: '',
    nextUrl: '',
  }),
  methods: {
    find() {
      const req = new Request(this.pokemonUrl)
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json()
        })
        .then((data) => {
          this.pokemon = data
          this.show = true
        })
        .catch((error) => {
          console.log(error)
        })
    },
    closeModal() {
      this.$emit('closeModal')
    },
    // nextPokemon() {
    //   // this.find(this.currentUrl + 1)
    //   this.find(this.currentUrl + '/' + 1)
    // },
    // prevPokemon() {
    //   if (this.currentUrl == 1) {
    //     return
    //   }
    //   this.find(this.currentUrl - 1)
    // },
  },
  created() {
    this.find()
  },
}
</script>

<style lang="scss">
@import '../styles/variables.scss';

.modal {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  width: 100%;
  height: 100vh;
  padding: 5%;
  position: fixed;
  overflow-y: scroll;
  top: 0;
  background: rgba(0, 0, 0, 0.6);
  cursor: default;
  .modal-view {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    width: 100%;
    max-width: 500px;
    padding: 2rem;
    position: relative;
    z-index: 2;
    border-radius: 15px;
    background-color: $header-color;
    box-shadow: 5px 8px 20px 2px $faded;
    .image {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100%;
      padding: 2rem;
      border: 2px solid green;
      border-radius: 15px;
      background-color: rgb(190, 209, 161);
      position: relative;
      z-index: 1;
      img {
        width: 150px;
      }
      .floor {
        width: 100%;
        height: 30%;
        border-radius: 12px;
        background-color: rgb(139, 173, 123);
        position: absolute;
        bottom: 0;
        z-index: -1;
      }
    }
    .info {
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-direction: column;
      width: 100%;
      margin: 1.5rem 0;
      padding: 0 0.75rem;
      .head-title {
        text-align: left;
        width: 100%;
        /* margin-bottom: 0.25rem; */
        h2 {
          font-size: 1.75rem;
          letter-spacing: 1px;
          text-transform: uppercase;
          color: $text-color;
        }
      }
      .head-info {
        width: 100%;
        margin: 0 0 2rem;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        .abilities {
          /* padding: 0.5rem; */
          /* flex-basis: 50%; */
          display: flex;
          flex-direction: column;
          flex-wrap: wrap;
          /* border: 1px solid $green; */
          .ability {
            h5 {
              font-size: 0.9rem;
              text-transform: uppercase;
              color: $green;
            }
          }
        }
        .properties {
          align-self: flex-end;
          .property {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            &:first-child {
              margin-bottom: 0.1rem;
            }
            p {
              margin-left: 0.25rem;
              font-size: 0.9rem;
              font-weight: 600;
              color: $green;
            }
          }
        }
      }
      h3 {
        color: $text-color;
        width: 100%;
      }
      .types {
        margin-bottom: 1rem;
      }
      .types,
      .weaknesses {
        display: flex;
        justify-content: flex-start;
        align-self: flex-start;
        flex-wrap: wrap;
        .type,
        .weakness {
          margin: 0.5rem 0.25rem;
          padding: 5px 10px;
          border-radius: 4px;
          color: $header-color;
          font-size: 1rem;
          letter-spacing: 2px;
          word-wrap: none;
          text-transform: uppercase;
          word-break: keep-all;
        }
        .type {
          background-color: $orange;
        }
        .weakness {
          background-color: $orange;
        }
      }
    }
    .sorry {
      text-align: center;
      width: 100%;
      margin: 1rem;
      h2 {
        color: $text-color;
        margin-bottom: 1rem;
      }
      p {
        margin: 0 auto;
        max-width: 220px;
        font-size: 1rem;
        padding: 1rem;
        border-radius: 5px;
        background-color: chartreuse;
        transition: 300ms ease;
        cursor: pointer;
        &:hover {
          background-color: $red;
        }
      }
    }
    .x {
      position: absolute;
      top: -10px;
      left: -10px;
      outline: none;
      border: none;
      height: 30px;
      width: 30px;
      font-size: 1.25rem;
      font-weight: bold;
      border-radius: 50%;
      color: black;
      background-color: $red;
      box-shadow: 2px 2px 10px 1px faded;
      cursor: pointer;
      transition: 300ms ease;
      &:hover {
        background-color: $blue;
      }
    }
    .background {
      height: 70%;
      width: 100%;
      border-radius: 15px;
      background-color: rgba(228, 212, 163, 0.7);
      position: absolute;
      left: 0;
      bottom: 0;
      z-index: -1;
    }
  }
}
</style>

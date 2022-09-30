<script>
import Modal from '../components/Modal.vue'

import {ref, watchEffect} from "vue";

const pokemonDetails2 = ref(null);
const API_URL = 'https://pokedex-api01.herokuapp.com/v1/pokedex/get/details?pokemonName='
export default {
  components: {
    Modal
  },
  data() {
    return {
      showModal: false,
    }
  },
  methods: {
    search() {
      const input = document.getElementById('fname').value
      console.log(input)
      watchEffect(async () => {
        pokemonDetails2.value = await (await fetch(API_URL + input, {
          mode: "cors", method: "GET", headers: {
            'Access-Control-Allow-Origin': '*'
          }
        })).json()
      })
      console.log(pokemonDetails2.value);
    }
  }
}

</script>

<template>
    <div class="about">
      <label for="fname">Pokemon Name:</label>
      <input type="text" id="fname" name="fname"><br><br><br>
      <button id="show-modal" @click="search() ; showModal = true">Search</button>
      <!--TODO : POPULATE THE SEARCH MODAL POPUP-->
    </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
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

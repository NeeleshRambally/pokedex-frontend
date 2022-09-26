<script>
import Modal from '../components/Modal.vue'

import {ref, watchEffect} from "vue";

const pokemonDetails2 = ref(null);
const API_URL = 'http://localhost:51183/v1/pokedex/get/details?pokemonName='
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
      watchEffect(async () => {
        pokemonDetails2.value = await (await fetch(API_URL + input, {
          mode: "cors", method: "GET", headers: {
            'Access-Control-Allow-Origin': '*'
          }
        })).json()
      })
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
    <Teleport to="body">
      <modal :show="showModal" @close="showModal = false">
        <template #header>
        </template>
        <template #body>
          <!--          <img :src="pokemonDetails2.imageURL" width="100" height="100"/>-->
          <h2>Type:</h2>
          <!--          <template v-for="(typeObj) in pokemonDetails.types" key="typeObj">-->
          <!--            <option :value="typeObj">-->
          <!--              <div :id="typeObj.typeName">{{ typeObj.typeName }}-->
          <!--              </div>-->
          <!--            </option>-->
          <!--          </template>-->
          <h2>Ability</h2>
          <!--          <template v-for="(abilityObj) in pokemonDetails.abbilities" key="abilityObj">-->
          <!--            <option :value="abilityObj" style="overflow-y: auto;overflow-x: hidden">-->
          <!--              <div :id="abilityObj.abbilityName">{{ abilityObj.abbilityName }}-->
          <!--              </div>-->
          <!--            </option>-->
          <!--          </template>-->
          <h2>Moves</h2>
          <div>
            <!--            <template v-for="(moveObj) in pokemonDetails.moves" key="moveObj">-->
            <!--              <option :value="moveObj" style="line-break: normal">-->
            <!--                {{ moveObj.moveName }}-->
            <!--              </option>-->
            <!--            </template>-->
          </div>
        </template>
      </modal>
    </Teleport>

  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>


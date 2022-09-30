<script setup>
import {onMounted, ref, watchEffect} from "vue";

const pokemonNameAndImageUrl = ref(null);
function getAllNamesAndImageUrl() {
  const API_URL = `https://pokedex-api01.herokuapp.com/v1/pokedex/get/nameAndImage?page=1&size=100`
  watchEffect(async () => {
    pokemonNameAndImageUrl.value = await (await fetch(API_URL, {
      mode: "cors", method: "GET", headers: {
        'Access-Control-Allow-Origin': '*'
      }
    })).json()
  })
  return true;
}

onMounted(() => {
  getAllNamesAndImageUrl();
})
</script>

<script>
import Modal from './Modal.vue'

const API_URL = 'https://pokedex-api01.herokuapp.com/v1/pokedex/get/details?pokemonName='
const pokemonDetails = ref(null);
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
    getDetailsByName(name) {
      watchEffect(async () => {
        pokemonDetails.value = await (await fetch(API_URL + name, {
          mode: "cors", method: "GET", headers: {
            'Access-Control-Allow-Origin': '*'
          }
        })).json()
      })
      console.log(pokemonDetails.value);
    }
  }
}

</script>
<template>
  <nav>
    <ul>
      <li>
        <template v-for="(obj) in pokemonNameAndImageUrl" :key="obj">
          <option :value="obj">
            <div :id="obj.name">{{ obj.name }}
            </div>
          </option>
          <img :src="obj.imageURL" width="100" height="100"/>
          <button id="show-modal" @click="getDetailsByName(obj.name) ; showModal = true">Details</button>
          <Teleport to="body">
            <modal :show="showModal" @close="showModal = false">
              <template #header>
                {{ pokemonDetails.name }}
              </template>
              <template #body>
                <img :src="pokemonDetails.imageURL" width="100" height="100"/>
                <h2>Type:</h2>
                <template v-for="(typeObj) in pokemonDetails.types" key="typeObj">
                  <option :value="typeObj">
                    <div :id="typeObj.typeName">{{ typeObj.typeName }}
                    </div>
                  </option>
                </template>
                <h2>Ability</h2>
                <template v-for="(abilityObj) in pokemonDetails.abbilities" key="abilityObj">
                  <option :value="abilityObj" style="overflow-y: auto;overflow-x: hidden">
                    <div :id="abilityObj.abbilityName">{{ abilityObj.abbilityName }}
                    </div>
                  </option>
                </template>
                <h2>Moves</h2>
                <div>
                  <template v-for="(moveObj) in pokemonDetails.moves" key="moveObj">
                    <option :value="moveObj" style="line-break: normal">
                      {{ moveObj.moveName }}
                    </option>
                  </template>
                </div>
              </template>
            </modal>
          </Teleport>
        </template>
      </li>
    </ul>
  </nav>
</template>
<style>
nav ul {
  height: 700px;
  width: 500%;
}

nav ul {
  overflow: hidden;
  overflow-y: scroll;
}
</style>



<template>
  <q-page class="flex flex-center">
    <div class="q-ma-md fit row wrap justify-center items-start ">
      <div class="col-6 col-sm-8 col-xs-12 col-md-6 col-lg-6 col-xl-6">
        <q-card class="my-card">
          <img src="https://camo.githubusercontent.com/90f24316b24433bf0a2778d95ca93ad264093645a71d2eb9f569cdbc34fbfc6d/68747470733a2f2f6372797374616c2d63646e322e6372797374616c636f6d6d657263652e636f6d2f70686f746f732f363333303536352f73706c61736842616e6e65725f706f6b656d6f6e2e6a7067">
          <q-card-section>
            <q-input bottom-slots v-model="text" label="Pokemon" counter maxlength="15" :dense="dense">
              <template v-slot:hint>
                Search for a pokemon
              </template>
            </q-input>
          </q-card-section>
          <q-card-section>
              <div class="flex text-primary" v-for="(pokemon, idx) in filterdPokemon" :key="idx">
                <router-link :to="`/pokemon/${urlIdLookup[pokemon.name]}`">
                  {{ pokemon.name }}
                </router-link>
              </div>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, reactive, toRefs, computed } from 'vue'

export default defineComponent({
  name: 'PageIndex',
  setup () {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      text: '',
      filterdPokemon: computed(() => updatePokemon())
    })

    function updatePokemon () {
      if (!state.text) {
        return []
      }

      return state.pokemons.filter((pokemon) => pokemon.name.includes(state.text))
    }

    fetch('https://pokeapi.co/api/v2/pokemon?offset?0').then(res => res.json()).then(data => {
      state.pokemons = data.results
      state.urlIdLookup = data.results.reduce((acc, cur, idx) => {
        acc = { ...acc, [cur.name]: idx + 1 }
        return acc
      })
    })

    return { ...toRefs(state) }
  }
})
</script>

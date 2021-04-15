<template>
  <q-page class="flex flex-center">
    <q-spinner-cube
      v-if="loading"
      color="primary"
      size="10em"
    />
    <div v-if="!loading" class="q-ma-md fit row wrap justify-center items-start ">
      <div class="col-6 col-sm-8 col-xs-12 col-md-6 col-lg-6 col-xl-6">
        <q-card class="my-card">
          <img src="https://camo.githubusercontent.com/90f24316b24433bf0a2778d95ca93ad264093645a71d2eb9f569cdbc34fbfc6d/68747470733a2f2f6372797374616c2d63646e322e6372797374616c636f6d6d657263652e636f6d2f70686f746f732f363333303536352f73706c61736842616e6e65725f706f6b656d6f6e2e6a7067">
          <q-card-section>
            <q-input bottom-slots v-model="text" label="Pokemon" counter maxlength="20" :dense="dense">
              <template v-slot:hint>
                Search for a pokemon
              </template>
            </q-input>
          </q-card-section>
          <q-card-section>
            <q-list bordered separator>
              <q-item :to="`/pokemon/${pokemon.name}`" v-for="(pokemon, idx) in filterdPokemon" :key="idx" clickable v-ripple>
                <q-item-section class="text-primary">{{ pokemon.name }}</q-item-section>
              </q-item>
            </q-list>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, reactive, toRefs, computed, ref } from 'vue'

export default defineComponent({
  name: 'PageIndex',
  setup () {
    const loading = ref(true)
    const state = reactive({
      pokemons: [],
      text: '',
      filterdPokemon: computed(() => updatePokemon())
    })

    function updatePokemon () {
      if (!state.text) {
        return []
      }

      return state.pokemons.filter((pokemon) => pokemon.name.includes(state.text.toLowerCase()))
    }

    fetch('https://pokeapi.co/api/v2/pokemon?offset=0&limit=1000').then(res => res.json()).then(data => {
      state.pokemons = data.results
    }).then(() => {
      loading.value = false
    })

    return { loading, ...toRefs(state) }
  }
})
</script>

<template>
  <q-page class="flex flex-center">
    <div v-if="pokemon" class="q-ma-md fit row wrap justify-center items-start ">
      <div class="col-6 col-sm-8 col-xs-12 col-md-6 col-lg-6 col-xl-6">
        <q-card class="my-card">
            <q-carousel
            swipeable
            transition-prev="scale"
            transition-next="scale"
            animated
            v-model="slide"
            arrows
            navigation
            infinite
            class="bg-primary"
            >
            <q-carousel-slide :name="1" class="column no-wrap flex-center">
                <div class="q-mt-md text-h2 text-white text-center">
                    {{ pokemon.name }}
                </div>
            </q-carousel-slide>
            <q-carousel-slide :name="2" :img-src="pokemon.sprites.front_shiny" />
            <q-carousel-slide :name="3" :img-src="pokemon.sprites.back_shiny" />
            </q-carousel>
          <q-card-section>
              <div class="flex flex-center">
                  <div class="text-h6" v-for="(type, idx) in pokemon.types" :key="idx">
                      {{type.type.name}}
                  </div>
              </div>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
import { useRoute } from 'vue-router'
import { defineComponent, reactive, toRefs, ref } from 'vue'

export default defineComponent({
  name: 'PageIndex',
  setup () {
    const slide = ref(1)
    const route = useRoute()

    const state = reactive({
      pokemon: null
    })

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`).then(res => res.json()).then(data => {
      state.pokemon = data
    })

    return { slide, ...toRefs(state) }
  }
})
</script>

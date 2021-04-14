<template>
  <q-page class="flex flex-center">
    <div class="bg-red" v-if="!pokemon">
        <h1 class="text-white">POKEMON NOT FOUND</h1>
    </div>
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
              <div class="fit row wrap justify-start items-stretch ">
                  <!-- Type -->
                  <div class="q-mb-sm col-4 col-xs-12 col-sm-12 col-md-4 col-lg-4 col-xl-4 text-h6">
                    <q-card class="my-card">
                        <q-card-section>
                            <div class="text-h6">Type</div>
                        </q-card-section>

                        <q-separator />
                        <q-card-section>
                            <div v-for="(type, idx) in pokemon.types" :key="idx">
                                <span> {{type.type.name}} </span>
                            </div>
                        </q-card-section>
                    </q-card>
                  </div>
                  <!-- Abilities -->
                  <div class="q-mb-sm col-4 col-xs-12 col-sm-12 col-md-4 col-lg-4 col-xl-4 text-h6">
                    <q-card class="my-card">
                        <q-card-section>
                            <div class="text-h6">Abilities</div>
                        </q-card-section>

                        <q-separator />

                        <q-card-section>
                            <div v-for="(ability, idx) in pokemon.abilities" :key="idx">
                                <span>{{ability.ability.name}}</span>
                            </div>
                        </q-card-section>
                    </q-card>
                  </div>
                  <!-- Stats --->
                  <div class="q-mb-sm col-4 col-xs-12 col-sm-12 col-md-4 col-lg-4 col-xl-4 text-h6">
                    <q-card class="my-card">
                        <q-card-section>
                            <div class="text-h6">Weight and Height</div>
                        </q-card-section>

                        <q-separator />

                        <q-card-section>
                            <div>
                                <span>{{pokemon.height * 10}} cm</span>
                            </div>
                            <div>
                                <span>{{pokemon.weight / 10}} kg</span>
                            </div>
                        </q-card-section>
                    </q-card>
                  </div>
                    <q-btn to="/" color="secondary q-mt-xl">
                        <q-icon color="primary" left size="2em" name="arrow_back" />
                        <div class="text-primary">Home</div>
                    </q-btn>
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

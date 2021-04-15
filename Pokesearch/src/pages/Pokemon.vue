<template>
  <q-page class="flex flex-center">
    <!-- Loading Spinner -->
    <q-spinner-cube
      v-if="loading"
      color="primary"
      size="10em"
    />
    <!-- End Loading Spinner -->
    <div v-if="pokemon" class="q-ma-md fit row wrap justify-center items-start ">
      <div class="col-6 col-sm-8 col-xs-12 col-md-6 col-lg-6 col-xl-6">
        <!-- Carousel -->
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
             <!-- End Carousel -->
          <q-card-section>
              <div class="fit row wrap flex-center items-stretch ">
                  <!-- Type -->
                  <div class="q-mb-sm q-mr-md q-ml-md col-12 col-xs-12 col-sm-12 col-md-12 col-lg-12 col-xl-12 text-h6">
                    <q-card class="my-card">
                        <q-card-section>
                            <div class="text-h6">
                              <q-icon name="catching_pokemon"/>
                              Type
                            </div>
                        </q-card-section>

                        <q-separator />
                        <q-card-section>
                            <div v-for="(type, idx) in pokemon.types" :key="idx">
                                <span> {{type.type.name}} </span>
                            </div>
                        </q-card-section>
                    </q-card>
                  </div>
                  <!-- End Type -->
                  <!-- Abilities -->
                  <div class="q-mb-sm q-mr-md q-ml-md col-12 col-xs-12 col-sm-12 col-md-12 col-lg-12 col-xl-12 text-h6">
                    <q-card class="my-card">
                        <q-card-section>
                            <div class="text-h6">
                              <q-icon name="bolt"/>
                              Abilities
                            </div>
                        </q-card-section>

                        <q-separator />

                        <q-card-section>
                            <div v-for="(ability, idx) in pokemon.abilities" :key="idx">
                                <span>{{ability.ability.name}}</span>
                            </div>
                        </q-card-section>
                    </q-card>
                  </div>
                  <!-- End Abilities -->
                  <!-- Stats --->
                  <div class="q-mb-sm q-mr-md q-ml-md col-12 col-xs-12 col-sm-12 col-md-12 col-lg-12 col-xl-12 text-h6">
                    <q-card class="my-card">
                        <q-card-section>
                            <div class="text-h6">
                              <q-icon name="health_and_safety"/>
                              Height and Weight
                            </div>
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
              </div>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
import { useRoute, useRouter } from 'vue-router'
import { defineComponent, reactive, toRefs, ref } from 'vue'

export default defineComponent({
  name: 'PageIndex',
  setup () {
    //  Router
    const router = useRouter()
    const route = useRoute()
    //  Loading
    const loading = ref(true)
    //  Carousel
    const slide = ref(1)
    //  State
    const state = reactive({
      pokemon: null
    })

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`).then(res => {
      if (res.ok) {
        console.log('Pokemon found')
        return res
      } else {
        return Promise.reject('Pokemon not found')
      }
    }).then(res => res.json()).then(data => {
      state.pokemon = data
    }).then(() => {
      loading.value = false
    }).catch(() => {
      router.push('/pokemon')
    })

    return { loading, slide, ...toRefs(state) }
  }
})
</script>

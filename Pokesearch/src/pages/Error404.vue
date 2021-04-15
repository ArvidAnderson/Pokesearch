<template>
  <div class="fullscreen bg-primary text-white text-center q-pa-md flex flex-center">
    <div>
      <div style="font-size: 30vh">
        404
      </div>

      <div class="text-h2 text-dark" style="opacity:.4">
        Oops. Lost in the wilderness...
      </div>

      <q-btn
        class="q-mt-xl q-pl-xl q-pr-xl"
        color="white"
        text-color="dark"
        unelevated
        to="/"
        label="Back to search"
        no-caps
      />
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Error404'
})
</script>

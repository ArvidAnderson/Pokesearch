<template>
  <q-layout view="hHh lpR fFf">

    <q-header elevated class="q-pa-sm bg-primary text-white" height-hint="98">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.pixabay.com/photo/2016/07/23/13/18/pokemon-1536847_1280.png">
          </q-avatar>
          <strong class="q-ml-md">Pokesearch</strong>
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>

<script>
export default {
}
</script>

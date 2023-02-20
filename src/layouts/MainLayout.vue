<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="bg-grey-10">

        <q-toolbar-title class="text-center " >
          Todo App
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
  name: 'MainLayout',
  components: {

  },
  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: []
    }
  }
}
</script>

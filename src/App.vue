<template>
  <v-app>
    <v-content>
      <main-window/>
    </v-content>
  </v-app>
</template>

<script>
import MainWindow from './components/MainWindow'

export default {
  name: 'App',
  components: {
    'main-window': MainWindow
  },
  data: () => ({
    //
  }),
};
</script>

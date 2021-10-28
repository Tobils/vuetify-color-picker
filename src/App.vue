<template>
  <v-app>
    <v-main>
      <ColorPicker/>
    </v-main>
  </v-app>
</template>

<script>
import ColorPicker from './components/ColorPicker.vue'

export default {
  name: 'App',

  components: {
    ColorPicker
  },

  data: () => ({
    //
  }),
};
</script>

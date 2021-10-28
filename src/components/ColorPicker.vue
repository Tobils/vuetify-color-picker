<template>
  <div class="main">
    <div class="color-picker">
      <v-color-picker
        class="ma-2"
        v-model="color"
        :swatches="swatches"
        show-swatches
      ></v-color-picker>
        <span>{{colors}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "ColorPicker",
  data(){
      return {
          color: '',
          colors: []
      }
  },
  watch:{
      color: function(){
          console.log(this.color);
          this.colors.push(this.color)
      }
  }
};
</script>

<style scoped>
.main {
  height: 100vh;
  display: flex;
  flex: 1;
  justify-content: center;
  align-content: center;
  align-items: center;
}
</style>

<template>
  <div class="container">
    <Form @draw="getCoords" />
    <Canvas ref="canvas" :data="coords" />
  </div>
</template>
<script>
import Canvas from "./components/Canvas.vue";
import Form from "./components/Form.vue";

export default {
  name: "App",
  data() {
    return {
      coords: Array(),
    };
  },
  components: {
    Form,
    Canvas,
  },
  methods: {
    getCoords(data) {
      this.coords = data;
      this.$refs.canvas.redraw();
    },
  },
};
</script>

<style>
body,
html {
  margin: 0;
  background: #07afda;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
#app {
  font-family: "Courier New", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 60px; */
}
.container {
  display: flex;
}
</style>

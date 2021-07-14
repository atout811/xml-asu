<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <div class="hello">
      <div class="text" contenteditable="true"></div>
    </div>
    <div>
      <input type="file" @change="fileload" />
      <button @click="format">Format</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      txt: "",
    };
  },
  methods: {
    format() {
      console.log("fuck");
    },
    fileload(ev) {
      const file = ev.target.files[0];
      const reader = new FileReader();

      reader.onload = (e) => {
        this.txt = e.target.result;
      };
      reader.readAsText(file);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <div class="hello">
      <textarea
        cols="169"
        rows="42"
        spellcheck="false"
        v-model="txt"
      ></textarea>
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
      let parser = new DOMParser();
      let xmlDoc = parser.parseFromString(this.txt, "application/xml");
      console.log(xmlDoc);
      let errors = xmlDoc.getElementsByTagName("parsererror");
      if (errors.length > 0) {
        console.log(errors);
      }
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
.main {
  width: 90%;
  height: 90%;
  border: 1px black solid;
  text-align: left;
}
#main-area {
  visibility: hidden;
}
</style>
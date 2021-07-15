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
<<<<<<< HEAD
    };
=======
      new_txt: "",
    }
>>>>>>> a24cc056795f076dc8fea109b0bbeb9c0e962860
  },
  methods: {
    format() {
      let parser = new DOMParser();
      let xmlDoc = parser.parseFromString(this.txt, "application/xml");
      console.log(xmlDoc.childNodes);
      let errors = xmlDoc.getElementsByTagName("parsererror");
      if (errors.length > 0) {
        console.log(errors);
      }
      for (const el of xmlDoc.childNodes) {
        console.log("heey", el);
      }
<<<<<<< HEAD
=======
      this.prettify(xmlDoc.activeElement, 0);
      this.txt = this.new_txt;
      this.new_txt = "";

>>>>>>> a24cc056795f076dc8fea109b0bbeb9c0e962860
    },
    fileload(ev) {
      const file = ev.target.files[0];
      const reader = new FileReader();

      reader.onload = (e) => {
        this.txt = e.target.result;
      };
      reader.readAsText(file);
    },
    prettify(doc, indentation){
      try{
          let attrs = "";
          for(let a of doc.attributes){
            attrs += ` ${a.name}="${a.textContent}"`
          }
          var tabs = "";
          for(let i=0; i<indentation; i++) {tabs += "\t"}

          this.new_txt += `${tabs}<${doc.tagName}${attrs}>\n`;
          for(let d of doc.children){
            this.prettify(d, indentation + 1);
          }
          if(doc.children.length == 0){
            this.new_txt += tabs + "\t" + doc.textContent + "\n"
          }
          this.new_txt += `${tabs}</${doc.tagName}>\n`
        }catch(e){
          console.log(doc);
          console.log(e);
        }
      }
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
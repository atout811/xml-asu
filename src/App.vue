<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <div class="hello">
      <div class="err" v-if="error">{{ error[0].innerText }}</div>
      <AceEditor
        v-model="content"
        @init="editorInit"
        lang="javascript"
        theme="monokai"
        width="100%"
        height="200px"
        :options="{
          enableBasicAutocompletion: true,
          enableLiveAutocompletion: true,
          fontSize: 14,
          highlightActiveLine: true,
          enableSnippets: true,
          showLineNumbers: true,
          tabSize: 2,
          showPrintMargin: false,
          showGutter: true,
        }"
        :commands="[
          {
            name: 'save',
            bindKey: { win: 'Ctrl-s', mac: 'Command-s' },
            exec: dataSumit,
            readOnly: true,
          },
        ]"
      />
    </div>
    <div>
      <input type="file" @change="fileload" />
      <button @click="format">Format</button>
    </div>
  </div>
</template>

<script>
import AceEditor from "vuejs-ace-editor";
export default {
  components: {
    AceEditor,
  },
  name: "App",
  data() {
    return {
      content: "",
      new_content: "",
      error: "",
    };
  },
  methods: {
    format() {
      let parser = new DOMParser();
      let xmlDoc = parser.parseFromString(this.content, "application/xml");
      console.log(xmlDoc);
      let errors = xmlDoc.getElementsByTagName("parsererror");
      if (errors.length > 0) {
        this.error = errors;
        console.log(errors);
      } else {
        this.error = "";
        console.log("heey", xmlDoc);
        for (let d of xmlDoc.children) {
          this.prettify(d, 0);
        }
        this.content = this.new_content;
        this.new_content = "";
      }
    },
    fileload(ev) {
      const file = ev.target.files[0];
      const reader = new FileReader();

      reader.onload = (e) => {
        this.content = e.target.result;
      };
      reader.readAsText(file);
    },
    prettify(doc, indentation) {
      try {
        var attrs = "";
        for (let a of doc.attributes) {
          attrs += ` ${a.name}="${a.textContent}"`;
        }
        var tabs = "";
        for (let i = 0; i < indentation; i++) {
          tabs += "\t";
        }

        this.new_content += `${tabs}<${doc.tagName}${attrs}>\n`;
        for (let d of doc.children) {
          this.prettify(d, indentation + 1);
        }
        if (doc.children.length == 0) {
          this.new_content += tabs + "\t" + doc.textContent + "\n";
        }
        this.new_content += `${tabs}</${doc.tagName}>\n`;
      } catch (e) {
        console.log(doc);
        console.log(e);
      }
    },
    dataSumit() {
      //code here
    },
    editorInit: function () {
      require("brace/ext/language_tools"); //language extension prerequsite...
      require("brace/mode/html");
      require("brace/mode/xml"); //language
      require("brace/mode/less");
      require("brace/theme/monokai");
      require("brace/snippets/javascript"); //snippet
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
.err {
  color: red;
}
#main-area {
  visibility: hidden;
}
</style>
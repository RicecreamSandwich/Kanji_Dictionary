<template>
  <div id="app">
    <div id="search" style="width:50%; height:100px; margin:auto;background-color:orange;">
      <h1 style="float:left;margin-left:20px;margin-top:0px;font-size:50pt">字引</h1>
      <input placeholder="Enter Kana | English | Kanji" v-on:change="resetVariables" v-model="inputText" style="float:right;margin-top:35px;margin-right:20px;"/>
      <button class="btn btn-primary" v-on:click="getKanjiResult" style="float:right;margin-top:35px;">Search</button>
    </div>
    <br/>
    <!-- {{results["kanji"]["character"]}} -->
    <!--{{results}}-->
    <!--{{results2[0]}}-->
    <div>
      <div v-for="r in results2" v-bind:key="r.kanji" style="width:50%; margin:auto;">
        <div style="width:100%;height:200px;background-color:green;margin-bottom:30px;min-width:400px;max-width:800px;">
          <div id="k-char" style="width:34%;height:100px; position:relative;background-color:#ffff66;padding:0;">
                <h1 style="font-size:50pt;">{{r.kanji.character}}</h1>
          </div>
		
          <div id="k-eng" style="width:66%;height:50px;background-color:#fdad5c;float:right;margin-top:-100px;">
            <h2 style="margin-top:8px;">{{r.kanji.meaning.english}}</h2>
          </div>
		
          <div id="k-str" style="width:66%;height:50px;background-color:#fdad5c;float:right;margin-top:-50px;">
            <h2 style="margin-top:8px;"># of Strokes: {{r.kanji.strokes.count}}</h2>
          </div>

          <div id="radical" style="width:100%;height:50px;background-color:#fdad5c;margin-top:-20px;">
            <h2 style="margin-top:20px;">Radical Info: Character: {{r.radical.character}} Strokes: {{r.radical.strokes}} Kodansha: {{r.references.kodansha}}</h2>
          </div>

          <div id="examples" style="width:100%;height:50px;background-color:#fdad5c;margin-top:0px;margin-bottom:20px;">
            <h2 style="margin-top:0px;">Example: {{r.examples[0].japanese}}  {{r.examples[0].meaning.english}}</h2>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
import axios from 'axios'
import Vue from 'vue'
import { get } from 'http'

Vue.prototype.$axios = axios

var authKey = 'ec1ff2d3c3msh3461788e5f61e1ep1931b7jsn7b03c5b46c4c';
var config = {
  headers: {'X-RapidAPI-Host': 'kanjialive-api.p.rapidapi.com', 'X-RapidAPI-Key': 'ec1ff2d3c3msh3461788e5f61e1ep1931b7jsn7b03c5b46c4c'}
};
var url = 'https://kanjialive-api.p.rapidapi.com/api/public/search/'


export default{
  el: '#app',
  data() {
    return {
      results: [],
      results2: [],
      inputText : 'はな',
      count : 0
    }
  },
  /*mounted () {
    this.$axios
      .get(url.concat(this.inputText), config)
      //.get('https://kanjialive-api.p.rapidapi.com/api/public/kanji/all', config)
      .then(response => {
        this.results = response.data
      });
  }*/
  methods: {
    produceResult(){
        this.$axios
        //.get("https://kanjialive-api.p.rapidapi.com/api/public/kanji/".concat(this.inputText), config)
        .get(url.concat(this.inputText), config)
        .then(response => {
        this.results = response.data
      });
    },
    getKanjiResult(){
      this.$axios
        .get(url.concat(this.inputText), config)
        .then(response => {
        this.results = response.data
        }); 
      for (let i = 0; i < this.results.length; i++)
      this.$axios
        .get("https://kanjialive-api.p.rapidapi.com/api/public/kanji/".concat(this.results[i]["kanji"]["character"]), config)
        .then(response => {
          if(this.count==2)
            this.results2.push(response.data);
        });
      this.count = this.count + 1;
    },
    resetVariables()
    {
      this.count = 0;
      this.results = [];
      this.results2 = [];
    }
  }
}


</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  background-color: #20801B;
}
</style>

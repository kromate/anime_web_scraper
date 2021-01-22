<template>
  <div class="container">
    <div class="fieldInput">
      <input type="text" v-model="searchedText" />
      <button @click="fetch()">Search</button>
    </div>
    <div>
      <a v-for="(n, index) in searchArray" :key="index" :href="n.href">{{ n.name }}</a>
    </div>
  </div>
</template>

<script>
const cheerio = require("cheerio");
export default {
  name: "App",
  data() {
    return {
      searchedText: "nana",
      data: "",
      searchArray: [],
    };
  },
  methods: {
    fetch() {
      this.searchArray = [];
      fetch(
        `https://ajax.gogocdn.net/site/loadAjaxSearch?keyword=${this.searchedText}&id=-1&link_web=https%3A%2F%2Fgogoanime.so%2F`,
      )
        .then((response) => response.json())
        .then((data) => {
          // console.log(data.content);
          const $ = cheerio.load(data.content);
          $("a").each((i, elem) => {
            console.log(elem);
            const href = elem.attribs.href;
            const name = elem.children[1].data;

            this.searchArray.push({ name: name, href: href });
          });
          console.log(this.searchArray[1]);
          // const fi = $("a");
          // console.log(fi);

          this.data = data.content;
        });
    },
  },

  mounted() {
    this.fetch();
  },
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
input {
  height: 25px;
  width: 214px;
  padding: 6px 15px;
  border-radius: 5px;
  outline: none;
  border: none;
  background: #dddfe2;
  color: #3f4753;
  font-size: 14px;
  font-weight: bold;
}
button {
  padding: 6px 15px;
  margin: 0px 5px;
}

a {
  display: block;
  margin: 5px;
  text-decoration: none;
  background: burlywood;
  text-align: center;
  color: black;
  padding: 4px;
}
</style>

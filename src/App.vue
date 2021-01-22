<template>
  <div class="container">
    <div class="fieldInput">
      <input type="text" />
      <button>Search</button>
    </div>

    <a v-for="(n, index) in searchArray" :key="index" :href="n.href">{{ n.name }}</a>
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
      fetch(
        `https://ajax.gogocdn.net/site/loadAjaxSearch?keyword=${this.searchedText}&id=-1&link_web=https%3A%2F%2Fgogoanime.so%2F`,
      )
        .then((response) => response.json())
        .then((data) => {
          // console.log(data.content);
          const $ = cheerio.load(data.content);
          $("a").each((i, elem) => {
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
input {
  width: 60% !important;
  background: #ffffff7a;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  margin: 0px 9px;
  margin-right: 3rem;
  font-weight: 550;
}
</style>

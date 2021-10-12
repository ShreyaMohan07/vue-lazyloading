<template>
  <h1>Alphabetical Episode Listing</h1>
  <div class="container-main">
    <div
      class="container-section"
      v-for="(seasonData, heading, index) in groupbyAlphabets"
      :key="index"
    >
      <h2>{{ heading }}</h2>
      <EpisodeDesc :episodeLists="seasonData">
      </EpisodeDesc>
    </div>
  </div>
</template>

<script>
import data from "@/data.json";
import EpisodeDesc from "./EpisodeDesc.vue";
export default {
  components: { EpisodeDesc },
  name: "home",
  data() {
    return {
      episodeLists: [],
    };
  },
  computed: {
    
    groupbyAlphabets() {
      let list1 = {};
      let i = 65;
      let j = 91;
      let alphabets = [];
      for (let k = i; k < j; k++) {
        alphabets.push(String.fromCharCode(k));
      }
      console.log("new list||", alphabets);
      this.episodeLists.forEach((ele) => {
        let splitName = ele.name.split(" ");
        let e = splitName[1].split("");
        for (let c = 0; c < 26; c++) {
          if (alphabets[c] === e[0]) {
            if (Object.keys(list1).includes(alphabets[c])) {
              list1[alphabets[c]] = [...list1[alphabets[c]], ele];
              break;
            } else {
              list1[alphabets[c]] = [ele];
            }
          }
        }
      });
      console.log("list items||", list1);
      list1=Object.entries(list1).sort((a, b) => {
        if (b > a) {
          return -1;
        } else if (b < a) {
          return 1;
        } else return 0;
      });
      list1=Object.fromEntries(list1);
      console.log("sorted items||", list1);
      return list1;
    },
  },
  created() {
    this.episodeLists = data && data._embedded && data._embedded.episodes;
    console.log("created||", this.episodeLists);
  },

  methods: {},
};
</script>

<style>
</style>
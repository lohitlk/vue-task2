<template>
  <div >
    <h2 class="title">LazyLoading</h2>
    <div
      
      v-for="(seasonData, head, index) in groupbyAlphabets"
      :key="index"
    >
      <h2 class="head">{{ head }}</h2>
      <EpisodeDesc :episodeLists="seasonData" />
    </div>
  </div>
</template>

<script>
import data from "@/bigbang.json";
import EpisodeDesc from "./EpisodeDesc.vue";
export default {
  components: {
    EpisodeDesc,
  },
  name: "home",
  data() {
    return {
      episodeLists: [],
    };
  },
  computed: {
    // groupbySeason() {
    //   let list = {};
    //   this.episodeLists.forEach((ele) => {
    //     if (Object.keys(list).includes(ele.season.toString())) {
    //       list[ele.season] = [...list[ele.season], ele];
    //     } else {
    //       list[ele.season] = [ele];
    //     }
    //   });
    //   console.log(list);
    //   return list;
    // },

    // alphabets(){

    //   return alphabets;
    // },

    groupbyAlphabets() {
      let list1 = {};
      let i = 65;
      let j = 91;
      let alphabets = [];
      for (let k = i; k < j; k++) {
        alphabets.push(String.fromCharCode(k));
      }
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
      console.log(list1);
      list1 = Object.entries(list1).sort((a, b) => {
        if (b > a) {
          return -1;
        } else if (b < a) {
          return 1;
        } else return 0;
      });
      list1 = Object.fromEntries(list1);
      console.log();
      return list1;
    },
  },
  created() {
    this.episodeLists = data && data._embedded && data._embedded.episodes;
  },

  methods: {},
};
</script>

<style>
.head {
  font-weight: bold;
  padding-bottom: 20px;
}
.title{
  float: left;
}
</style>
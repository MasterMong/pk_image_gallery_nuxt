<template>
  <div class="ml-3 mr-3">
    <center class="mt-2">
      <h1 class="text-primary">คลังภาพกิจกรรมโรงเรียนภูเขียว</h1>
    </center>
    <div class="mt-3 mb-3">
      <a
        href="https://forms.gle/tgKUcUEbfjQ1bfse7"
        class="btn btn btn-warning"
        target="_blank"
        >➕ ส่งภาพกิจกรรม</a
      >
      <a
        href="https://drive.google.com/drive/folders/1LJ3f0eSPrm2fBWT6eBd-0ycrkQENznaX?usp=sharing"
        target="_blank"
        class="btn btn btn-primary"
        >📷 ภาพปี 2559 - 2562</a
      >
      <a
        href="https://drive.google.com/drive/folders/1U7OKA59dpmstrEUXdxLWPHiHM1BLf6DX?usp=sharing"
        target="_blank"
        class="btn btn btn-primary"
        >📷 ภาพปี 2563</a
      >
    </div>
    <b-row class="mb-3">
      <b-col cols="12">
        <b-form-input
          v-model="keyword"
          @click="keyword = ''"
          placeholder="🔎 ค้นหา"
        ></b-form-input>
      </b-col>
    </b-row>
    <div v-if="loaded">
      <b-table
        striped
        hover
        responsive
        :items="lists"
        :filter="keyword"
        :per-page="perPage"
        :current-page="currentPage"
        :sort-by.sync="sortBy"
        :fields="headTable"
        sort-icon-left
        :sort-desc.sync="sortDesc"
      >
        <template #cell(event)="data">
          <a
            v-bind:href="data.item.url"
            target="_blank"
            class=""
            style="color: black"
            >{{ data.value }}</a
          >
        </template>
        <template #cell(url)="data">
          <a v-bind:href="data.value" target="_blank" class="">👀 ดูภาพ</a>
        </template>
      </b-table>
      <div>
        Sorting By: <b>{{ sortBy }}</b
        >, Sort Direction:
        <b>{{ sortDesc ? "Descending" : "Ascending" }}</b>
      </div>
      <b-pagination
        v-model="currentPage"
        pills
        :total-rows="lists.length"
        :per-page="perPage"
      ></b-pagination>
    </div>

    <div v-else>
      <div class="d-flex justify-content-center mb-3 mt-3">
        <b-spinner variant="primary" label="Loading..."></b-spinner>
      </div>
    </div>
    <div id="disqus_thread"></div>
    <script>
      (function () {
        // DON'T EDIT BELOW THIS LINE
        var d = document,
          s = d.createElement("script");
        s.src = "https://gallery-phukhieo.disqus.com/embed.js";
        s.setAttribute("data-timestamp", +new Date());
        (d.head || d.body).appendChild(s);
      })();
    </script>
    <noscript
      >Please enable JavaScript to view the
      <a href="https://disqus.com/?ref_noscript"
        >comments powered by Disqus.</a
      ></noscript
    >
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: [],
      loaded: false,
      lists: [],
      imageSource: "",
      currentPage: 1,
      url:
        "https://docs.google.com/spreadsheets/d/e/2PACX-1vQDt9V4mtLkAPWNjfxXvfUIkYvo-nZ2nmricNvCqbVGvSGcAy9e8v7VYYv7zUsBeqaf04n_upel-Ob4/pub?gid=0&single=true&output=csv",
      sortBy: "",
      sortDesc: true,
      keyword: "",
      perPage: 80,
      headTable: [
        // { key: 'ref', label: 'Ref' },
        { key: "event", label: "ปี-เดือน-วัน กิจกรรม", sortable: true },
        { key: "url", label: "ลิงค์", sortable: true },
      ],
    };
  },
  methods: {
    loadAPI() {
      const vm = this;
      console.log(
        "%c" + "Loading list",
        "font-size: 16px;color:#2ecc71; background-color:#34495e;"
      );
      this.$axios.get(vm.url).then(function (response) {
        // console.log(response)
        vm.loaded = true;
        vm.lists = vm.csvJSON(response.data);
        console.log(
          "%c" + "Loading success",
          "font-size: 16px;color:#2ecc71; background-color:#34495e;"
        );
        console.log(vm.lists);
      });
    },
    csvJSON(csv) {
      const lines = csv.split("\n");
      const result = [];
      // const headers = lines[0].split(',')
      const headers = ["event", "url"];
      let empty = false;

      for (let i = 1; i < lines.length; i++) {
        if (!lines[i]) {
          continue;
        }
        const obj = {};
        const currentline = lines[i].split(",");

        for (let j = 0; j < headers.length; j++) {
          obj[headers[j]] = currentline[j];
          if (currentline[2] === "") {
            empty = true;
          }
        }
        if (!empty) {
          result.push(obj);
        }
      }
      return result;
    },
  },
  mounted() {
    console.log(
      "%c" + "Mount success",
      "font-size: 16px;color:#2ecc71; background-color:#34495e;"
    );
    this.loadAPI();
  },
};
</script>

<style>
a,
a:hover {
  text-decoration: none;
}
</style>

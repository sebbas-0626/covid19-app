<template>
  <main class="text-black" v-if="!loading">
    <DataTitle v-bing:dataDate="dataDate" v-bind:text="title" />
    <DataBoxes v-bind:stats="status" />

    <CountrySelect
      v-on:get-country="getCountryData"
      v-bind:countries="countries"
    />
    <button
      class="p-3 mt-10 text-white bg-green-700 rounded focus:outline-none hover:bg-green-600"
    >
      Clear Country
    </button>
  </main>
  <main class="flex flex-col justify-center text-center align-center" v-else>
    <div class="mt-10 mb-6 text-3xl text-gray-500">Fetching Data</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="" />
  </main>
</template>

<script>
import DataTitle from "@/components/DataTitle.vue";
import DataBoxes from "@/components/DataBoxes.vue";
import CountrySelect from "../components/CountrySelect.vue";

export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: "",
      status: {},
      countries: [],
      //loadingImage: require("../assets/hourglass.gif"),
    };
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");
      const data = await res.json();
      console.log(data);
      return data;
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
  },
  async created() {
    const data = await this.fetchCovidData();
    this.dataDate = data.dataDate;
    this.status = data.Global;
    this.countries = data.countries;
    this.loading = false;
  },
};
</script>

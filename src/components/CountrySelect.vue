<template>
  <select
    v-on:change="onchange"
    v-model="selected"
    class="block w-full p-3 mt-10 border rounded form-select"
  >
    <option value="0">Select Country</option>
    <option v-for="country in countries" v-bind:value="country.ID">
      {{ country.Country }}
    </option>
  </select>
</template>

<script>
export default {
  name: "CountrySelect",
  props: ["countries"],
  data() {
    return {
      selected: 0,
    };
  },
  methods: {
    onchange() {
      const country = this.countries.find((item) => item.ID === this.selected);
      console.log(country)
      this.$emit("get-country, country");
    },
  },
};
</script>

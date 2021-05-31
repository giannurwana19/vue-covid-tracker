<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="getCountryData" :countries="countries" />
    <button
      v-if="stats.Country"
      @click="clearCountry"
      class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600"
    >
      Clear Country
    </button>
  </main>

  <main v-else class="flex flex-col justify-center text-center">
    <div class="text-gray text-3xl mt-10 mb-6">Fetcing data</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="loading" />
  </main>
</template>

<script>
import axios from 'axios';
import DataTitle from '../components/DataTitle.vue';
import DataBoxes from '../components/DataBoxes.vue';
import CountrySelect from '../components/CountrySelect.vue';

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },

  data: function() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/spinner.gif'),
    };
  },

  methods: {
    fetchCovidData: async function() {
      const { data } = await axios.get('https://api.covid19api.com/summary');

      console.log(data);
      return data;
    },

    getCountryData: function(country) {
      this.stats = country;
      this.title = country.Country;
    },

    clearCountry: async function() {
      this.loading = true;
      const data = await this.fetchCovidData();

      this.title = 'Global';
      this.stats = data.Global;
      this.loading = false;
    },
  },
  async created() {
    const data = await this.fetchCovidData();

    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  },
};
</script>

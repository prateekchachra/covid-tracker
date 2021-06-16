<template>

  <Header />
  <div class="flex-col align-center justify-center text-center mb-48 m-12">
  <main v-if="!loading" >
    <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats"/>
    <CountrySelect :countries="countries" @get-country="getCountryData"/>
     <button @click="clearCountryData" v-if="stats.Country" class="bg-green-700 text-white rounded p-3 mt-10
     focus:outline-none hover:bg-green-600">
        Clear Country
     </button>
  </main>

  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data

    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="Loading"/>
      </main>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import DataTitle from './components/DataTitle.vue'
import DataBoxes from './components/DataBoxes.vue'
import CountrySelect from './components/CountrySelect.vue'

export default {
  name: 'App',
  components: {
    Header,
    DataTitle,
    DataBoxes,
    CountrySelect
  },
   methods: {
    async fetchCovidData(){
      const res = await fetch('https://api.covid19api.com/summary');
      const data = await res.json();
      return data;
    },
    getCountryData(country){
      this.stats = country;
      this.title = country.Country;
    },
    async clearCountryData(){
      this.loading = true;
      const data = await this.fetchCovidData();
      this.title = 'Global';
      this.stats = data.Global;
      this.loading = false;
    }
  },
  data(){
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      status: {},
      countries: [],
      loadingImage: require('./assets/hourglass.gif')}
  },
  async created(){
    const data = await this.fetchCovidData();
    const {Date, Global, Countries} = data;
    this.dataDate = Date;
    this.stats = Global;
    this.countries = Countries;
    this.loading = false;

}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>

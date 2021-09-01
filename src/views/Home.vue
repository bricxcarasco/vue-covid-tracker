<template>
  <main v-if="!loading">
    <data-title :text="title" :dataDate="dataDate"/>
    <data-boxes :stats="stats"/>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadImage" class="w-24 m-auto" alt="">
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'

export default {
  name: 'Home',
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadImage: require('../assets/hourglass.gif')
    }
  },
  components: {
    DataTitle, DataBoxes
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    }
  },
  async created() {
    const data = await this.fetchCovidData();
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.countries
    this.loading = false

    console.log(data);
  }
}
</script>

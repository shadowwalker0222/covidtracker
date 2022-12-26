<template>
  <main v-if="!loading">
    <DataTitle :dataDate="dataDate" :text="title" />

    <DataBoxes :stats="status" />

    <CountrySelect :countries="countries" @get-country="getCountryData" />

    <button
      v-if="status.Country"
      class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600"
      @click="clearCountryData"
    >
      Clear Country
    </button>
    <br>
    <br>
    <p>
    <b>
    Frequesntly ask question:
    </b>
    </p>
    <br>
     <p>
    <b>
    What are coronavirus and what is covid 19?
    </b>
    </p>
    <p>
    Coronaviruses (CoV) are a large family of viruses causing illnesses ranging from the common cold to pneumonia (a more severe lung infection). COVID-19 is the infectious disease caused by a strain of coronavirus, SARS-CoV-2. It was declared a pandemic by the World Health Organization in March 2020 and has spread globally.
    </p>
    <br>
    <p>
    <b>
    What are the symptoms of COVID-19?
    </b>
    </p>
    <p>
    The symptoms of COVID-19 infection are similar to that of an acute respiratory infection or pneumonia. These symptoms include fever, cough, runny nose, sore throat, loss of taste or smell.
    </P>
    <br>
     <p>
    <b>
    Is the disease deadly? How is it compared to SARS?
    </b>
    </p>
    <p>
    Internationally, COVID-19 has been observed to cause severe disease and death in 2% to 3% of people with the infection, especially among the elderly and those with underlying health problems or compromised immune systems. Singapore’s mortality rate remains below the global average.
    </p>
    <br>
    <b>
    <p>
    Where do I get the latest information on the disease situation?
    </p>
    </b>
    <p>
    Health advisories and the latest information on the local disease situation are available on the World Health Organization website at <a class="text-blue-300" href="https://www.who.int/" target="_blank">www.who.int.</a>
    </p>
    <br>
    <b>
    <p>
    What API are you using?
    </p>
    </b>
    <p>
      API by
      <a class="text-blue-300" href="https://covid19api.com" target="_blank">covid19api.com</a>
    </p>



    
  </main>

  <main v-else class="flex flex-col align-center justify-center text-center">
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="require('../assets/hourglass.gif')" alt="" class="w-24 m-auto" />
  </main>
</template>

<script>
import CountrySelect from '@/components/CountrySelect';
import DataBoxes from '@/components/DataBoxes';
import DataTitle from '@/components/DataTitle';
import { ref } from 'vue';

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
  },
  setup () {
    const loading = ref(true);
    const title = ref('Global');
    const dataDate = ref('');
    const status = ref({});
    const countries = ref([]);

    const fetchCovidData = async () => {
      const res = await fetch('https://api.covid19api.com/summary');
      return await res.json();
    };

    const getCountryData = (country) => {
      status.value = country;
      title.value = country.Country;
    };

    const clearCountryData = async () => {
      loading.value = true;
      const data = await fetchCovidData();
      title.value = 'Global';
      status.value = data.Global;
      loading.value = false;
    };

    const baseSetup = async () => {
      const data = await fetchCovidData();

      dataDate.value = data.Date;
      status.value = data.Global;
      countries.value = data.Countries;
      loading.value = false;
    };

    baseSetup();

    return {
      loading,
      title,
      dataDate,
      status,
      countries,
      getCountryData,
      clearCountryData
    };
  }
};
</script>

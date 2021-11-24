<template>
  <div id="app" class="container d-flex flex-column justify-content-center align-items-center mt-5">
    <h3>Willkommen bei der Service Worker Untersuchung!</h3>
    <ButtonGet @get="fetchData"></ButtonGet>
    <CardView :employees="employees" @del="delEmployee"></CardView>
  </div>
</template>

<script>
import ButtonGet from '@/components/ButtonGet.vue';
import CardView from '@/components/CardView.vue';
import axios from 'axios';

export default {
  name: 'app',
  components: {
    ButtonGet,
    CardView,
  },
  data() {
    return {
      employees: [],
      serverAddress: process.env.VUE_APP_SERVER,
      show: false,
    };
  },
  created() {
    document.addEventListener('swUpdated', this.updateAvailable, { once: true });
  },
  methods: {
    updateAvailable() {
      alert('Update vorhanden, bitte App neu starten!');
    },
    async getEmployees() {
      try {
        const { data } = await axios({
          url: this.serverAddress + '/employees',
          method: 'GET',
        });
        this.employees = data;
      } catch (error) {
        console.error(error);
      }
    },
    async delEmployee(e) {
      try {
        const { data } = await axios({
          url: this.serverAddress + `employees/${e}`,
          method: 'DELETE',
        });
        this.employees = data;
        this.getEmployees();
      } catch (error) {
        console.error(error);
      }
    },
    fetchData() {
      console.log('fetchData called');
      this.getEmployees();
    },
    // delEmployee() {
    //   console.log('delEmployee called');
    // },
  },
};
</script>

<style></style>

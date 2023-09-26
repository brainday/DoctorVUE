<template>
  <ul>
    <li v-for="item in shedule" :key="item.id">
      {{ item }}
    </li>
  </ul>
</template>

<script>

import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      oidMO: '1.2.643.5.1.13.13.12.2.72.7326',
      shedule: []
    }
  },
  methods: {
    async userLogin() {
      await axios.post('http://127.0.0.1:5000/api/auth/signin', {
        username: 'admin',
        password: 'admin'
    })
    .then((response) => {
      let parsed = JSON.stringify(response.data.accessToken);
      localStorage.setItem('x-access-token', parsed);
      this.dataShedule();
    })
    .catch((error) => {
      console.log(error);
      localStorage.removeItem('x-access-token');
    });
    },

    async dataShedule() {

      let headers = {
        'x-access-token': JSON.parse(localStorage.getItem('x-access-token'))
      };

      const oidMO = this.oidMO;
      const url = `http://127.0.0.1:5000/api/data/${oidMO}`

      await axios.get(url, { headers })
    .then((response) => {
      console.log(response);
      this.shedule = response.data
      })
      .catch((error) => {
        console.log(error);
      });
      },
  },
  mounted() {
      this.userLogin()
    }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

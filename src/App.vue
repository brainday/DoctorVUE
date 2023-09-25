<template>
  <ul>
    <li v-for="item in shedule" :key="item.id">
      {{ item.departments[0].name }}
    </li>
  </ul>
</template>

<script>

import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
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

      await axios.get('http://127.0.0.1:5000/api/data', { headers })
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

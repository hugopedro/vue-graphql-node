<template>
<v-container>
  <v-row>
    <v-col align="left">
      <router-link to="/domains">Voltar</router-link>
      <br/>
      <h2>{{ domain }}</h2>
      {{ domains }}
    </v-col>
  </v-row>
</v-container>
</template>

<script>

import axios from "axios/dist/axios"

export default {
  props: ["domain"],
  data(){
    return {
      domains: []
    }
  },
  created() {
    axios({
      url: "http://localhost:4000",
      method: "post",
      data: {
        query: `
          mutation ($name: String) {
            domains: generateDomain(name: $name) {
              name
              checkout
              available
            }
          }
        `,
        variables: {
          name: this.domain
        }
      }
    }).then(response => {
       const query = response.data;
       this.domains = query.data.domains;
    });
  }
}
</script>

<style>

</style>
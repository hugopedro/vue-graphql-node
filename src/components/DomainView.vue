<template>
<v-container>
  <v-row>
    <v-col align="left">
      <router-link to="/domains">Voltar</router-link>
      <br/>
      <h2>{{ domain }}</h2>
      <br />
    </v-col>
  </v-row>
  <v-row>
    <v-col>
        <li
          class="list-group-item"
          v-for="domain in domains"
          :key="domain.extension"
        >
          <v-col>
            {{ domain.extension }}
            <v-btn color="red" v-bind:href="domain.checkout" target="_blank"
              ><v-icon>mdi-cart</v-icon></v-btn
            >
          </v-col>
            <v-badge color="blue" :content="(domain.available ? 'Disponivel' : 'Não Disponivel')">
              </v-badge>
        </li>
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
              extension
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
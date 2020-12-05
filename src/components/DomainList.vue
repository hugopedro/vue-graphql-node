<template>
  <v-container>
    <v-row>
      <v-col>
        <app-item-list title="Prefixos" :items="prefixes" @addItem="addPrefix" @deleteItem="deletePrefix"/>
      </v-col>
      <v-col>
        <app-item-list title="Sufixos" :items="sufixes" @addItem="addSufix" @deleteItem="deleteSufix"/>
      </v-col>
    </v-row>
    <br/>
    <v-row>
    <h2>Domínios <v-badge color="blue" :content=domains.length></v-badge></h2>
      <v-col>
        <li class="list-group-item" v-for="domain in domains" :key="domain.name">
          <v-col>
            {{ domain.name }}
            <v-btn color="red" v-bind:href="domain.checkout" target="_blank"><v-icon>mdi-cart</v-icon></v-btn>
          </v-col>
        </li>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>

import AppItemList from './AppItemList.vue';

export default {
  name: "HelloWorld",

  components: {
    AppItemList
  },

  data: () => ({
    prefixes: ['Air', 'Jet', 'Flight'],
    sufixes: ['Hub', 'Station', 'Mart']
  }),
  methods: {
    addPrefix(prefix) {
      this.prefixes.push(prefix);
    },
    deletePrefix(prefix) {
      this.prefixes.splice(this.prefixes.indexOf(prefix), 1);
    },
    addSufix(sufix) {
      this.sufixes.push(sufix);
    },
    deleteSufix(sufix) {
      this.sufixes.splice(this.sufixes.indexOf(sufix), 1);    
    },
  },
  computed: { //o bom do computed é que só atualiza na hora certa, evitando gargalos
    domains() {
      const domains = [];
      for (const prefix of this.prefixes) {
        for (const sufix of this.sufixes) {
          const name = prefix + sufix;
          const url = name.toLowerCase();
          const checkout = `https://checkout.hostgator.com.br/?a=adds&sld=${url}&tld=.com.br`
          domains.push({
            name, checkout, 
          });
        }
      }
      return domains;
    }
  }
}
</script>

<style>

</style>

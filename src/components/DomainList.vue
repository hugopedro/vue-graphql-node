<template>
  <v-container>
    <v-row>
      <hello-world />
      <v-col>
        <app-item-list
          title="Prefixos"
          type="prefix"
          :items="items.prefix"
          @addItem="addItem"
          @deleteItem="deleteItem"
        />
      </v-col>
      <v-col>
        <app-item-list
          title="Sufixos"
          type="sufix"
          :items="items.sufix"
          @addItem="addItem"
          @deleteItem="deleteItem"
        />
      </v-col>
    </v-row>
    <br />
    <v-row>
      <h2>
        Domínios <v-badge color="blue" :content="domains.length"></v-badge>
      </h2>
      <v-col>
        <li
          class="list-group-item"
          v-for="domain in domains"
          :key="domain.name"
        >
          <v-col>
            {{ domain.name }}
            <v-btn color="red" v-bind:href="domain.checkout" target="_blank"
              ><v-icon>mdi-cart</v-icon></v-btn
            >
            &nbsp;
            <v-btn color="purple" @click="openDomain(domain)"><v-icon>mdi-magnify-plus-outline</v-icon></v-btn>
          </v-col>
            <v-badge color="blue" :content="(domain.available ? 'Disponivel' : 'Não Disponivel')">
              </v-badge>
        </li>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import AppItemList from "./AppItemList.vue";
import HelloWorld from './HelloWorld.vue';
import { mapState, mapActions } from "vuex";

export default {
  name: "DomainList",

  components: {
    AppItemList, HelloWorld
  },

  data: function () {
    return {};
  },
  methods: {/*
    addItem(item) {
      this.$store.dispatch("addItem", item);
    },
    deleteItem(item) {
      this.$store.dispatch("deleteItem", item);
    },
    getItems(type) {
			this.$store.dispatch("getItems", type);
    },
    generateDomains() {
      this.$store.dispatch("generateDomains");
    }, isso equivale ao codigo abaixo (depois da funcao openDomain) */
    openDomain(domain) {
      this.$router.push({
        path: `/domains/${domain.name}`
      })
    }, 
    ...mapActions(["addItem", "deleteItem", "getItems", "generateDomains"])
  },
  computed: {
    /*items() {
      return this.$store.state.items;
    },
    domains() {
      return this.$store.state.domains;
    },  isso equivale a linha abaixo */
    ...mapState(["items", "domains"])
  }
};
</script>

<style>
</style>

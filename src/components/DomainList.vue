<template>
  <v-container>
    <v-row>
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
          </v-col>
            <v-badge color="blue" :content="(domain.available ? 'Disponivel' : 'Não Disponivel')">
              </v-badge>
        </li>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios/dist/axios";
import AppItemList from "./AppItemList.vue";

export default {
  name: "HelloWorld",

  components: {
    AppItemList,
  },

  data: function () {
    return {
      items: {
        prefix: [],
        sufix: [],
      },
      domains: []
    };
  },
  methods: {
    addItem(item) {
      axios({
        // esse newPrefix é um apelido que estou dando ao saveItem
        url: "http://localhost:4000",
        method: "post",
        data: {
          query: `
            mutation ($item: ItemInput) {
              newItem: saveItem(item: $item) {
                id
                type
                description
              }
            }
          `,
          variables: {
            item
          },
        },
      }).then((response) => {
        const query = response.data;
        const newItem = query.data.newItem;
        this.items[item.type].push(newItem);
        this.generateDomains();
      })
    }, //deleted é um alias
    deleteItem(item) {
      axios({
        url: "http://localhost:4000",
        method: "post",
        data: {
          query: `
            mutation($id: Int) {
              deleted: deleteItem(id: $id)
            }
          `,
          variables: {
            id: item.id
          }
        }
      }).then(() => {
        this.items[item.type].splice(this.items[item.type].indexOf(item), 1);
        this.generateDomains();
      });
    },
    getItems(type) {
			return axios({
				url: "http://localhost:4000",
				method: "post",
				data: {
					query: `
						query ($type: String) {
							items: items (type: $type) {
								id
								type
								description
							}
						}
					`,
					variables: {
						type
					}
				}
			}).then((response) => {
				const query = response.data;
				this.items[type] = query.data.items;
			});
    },
    generateDomains() {
      axios({
        url: "http://localhost:4000",
        method: "post",
        data: {
          query: `
            mutation {
              domains: generateDomains {
                name
                checkout
                available
              }
            }
          `
        }
      }).then((response) => {
        const query = response.data;
        this.domains = query.data.domains; 
      });
    }
  },
  created() {
    Promise.all([
      this.getItems("prefix"),
      this.getItems("sufix")
    ]).then(() => {
      this.generateDomains();
    });
  },
};
</script>

<style>
</style>

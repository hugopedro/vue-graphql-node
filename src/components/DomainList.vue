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
        this.getItems(item.type);
      });
    },
    getItems(type) {
			axios({
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
  },
	computed: { //o bom do computed é que ele só atualiza na hora certa, evitando gargalos
		domains() {
			console.log("generating domains...");
			const domains = [];
			for (const prefix of this.items.prefix) {
				for (const sufix of this.items.sufix) {
					const name = prefix.description + sufix.description;
					const url = name.toLowerCase();
					const checkout = `https://checkout.hostgator.com.br/?a=add&sld=${url}&tld=.com.br`;
					domains.push({
						name,
						checkout
					});
				}
			}
			return domains;
		}
	},
  created() {
		this.getItems("prefix");
		this.getItems("sufix");
  },
};
</script>

<style>
</style>

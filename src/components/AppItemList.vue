<template>
  <v-row>
    <v-col>
        <h3>{{title}} <v-badge color="green" :content=items.length></v-badge></h3>
        <li class="list-group-item" v-for="item in items" :key="item.id">
          {{ item.description }}
          <div class="d-flex flex-row-reverse mb-16">
            <v-btn @click="deleteItem(item)" class="mx-2" fab dark small color="primary" > <v-icon dark> mdi-minus </v-icon></v-btn>
          </div>
        </li>
        <v-text-field input type="text" class="form-control" placeholder="Digite o item"
        v-model="description" v-on:keyup.enter="addItem(type, description)">
        </v-text-field>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "AppItemList",
  props: ["title","type", "items"],
  data() {
    return {
      description: ""
    }
  },
  methods: {
    addItem(type, description) {
			this.$emit("addItem", {
				type,
				description
			});
			this.description = "";
		},
		deleteItem(item) {
			this.$emit("deleteItem", item);
		}
  }
}; 
</script>

<style>
</style>
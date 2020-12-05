<template>
  <v-row>
    <v-col>
        <h3>{{title}} <v-badge color="green" :content=items.length></v-badge></h3>
        <li class="list-group-item" v-for="item in items" :key="item">
          {{ item }}
          <div class="d-flex flex-row-reverse mb-16">
            <v-btn @click="deleteItem(item)" class="mx-2" fab dark small color="primary" > <v-icon dark> mdi-minus </v-icon></v-btn>
          </div>
        </li>
        <v-text-field input type="text" class="form-control" placeholder="Digite o item"
        v-model="item" v-on:keyup.enter="addItem(item)">
        </v-text-field>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "AppItemList",
  props: ["title","items"],
  data() {
    return {
      item: ""
    }
  },
  methods: {
    addItem(item) {
      this.$emit("addItem", item);
      this.item = ""
    },
    deleteItem(item) {
      this.$emit("deleteItem", item);
      this.item = ""
    }
  }
}; 
</script>

<style>
</style>
<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12" class="slogan ma-0 pa-0">
        <h1>
          NameGator
        </h1>
        <br/>
        <h2 class="none">Gerador de nomes usando Vue.Js</h2>
        <br/>
        <br/>
        <br/>
      </v-col>
      <v-col class="main ma-0 pa-0">
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <h3>Prefixos <v-badge color="green" :content=prefixes.length></v-badge></h3>
        <li class="list-group-item" v-for="prefix in prefixes" :key="prefix">
          {{ prefix }}
          <div class="d-flex flex-row-reverse mb-16">
            <v-btn @click="deletePrefix(prefix)" class="mx-2" fab dark small color="primary" > <v-icon dark> mdi-minus </v-icon></v-btn>
          </div>
        </li>
        <v-text-field input type="text" class="form-control" placeholder="Digite o prefixo"
        v-model="prefix" v-on:keyup.enter="addPrefix(prefix)">
        </v-text-field>
      </v-col>
      <v-col>
      <h3>Sufixos <v-badge color="green" :content=sufixes.length></v-badge></h3>
        <li class="list-group-item" v-for="sufix in sufixes" :key="sufix">
          {{ sufix }}
            <div class="d-flex flex-row-reverse mb-16">
              <v-btn @click="deleteSufix(sufix)" class="mx-2" fab dark small color="primary" > <v-icon dark> mdi-minus </v-icon></v-btn>
          </div>
        </li>
        <v-text-field input type="text" class="form-control" placeholder="Digite o sufixo"
        v-model="sufix" v-on:keyup.enter="addSufix(sufix)">
        </v-text-field>
      </v-col>
    </v-row>
    <br/>
    <h2>Domínios <v-badge color="blue" :content=domains.length></v-badge></h2>
    <v-row>
      <v-col>
        <li class="list-group-item" v-for="domain in domains" :key="domain">
          {{ domain }}
        </li>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",

  data: () => ({
    prefix: "",
    sufix: "",
    prefixes: ['Air', 'Jet', 'Flight'],
    sufixes: ['Hub', 'Station', 'Mart'],
    domains: ['AirHub', 'AirStation', 'AirMart', 'JetHub', 'JetStation', 'JetMart', 'FlightHub', 'FlightStation', 'FlightMart']
  }),
  methods: {
    addPrefix(prefix) {
      this.prefixes.push(prefix);
      this.prefix = "";
      this.generate();
    },
    deletePrefix(prefix) {
      this.prefixes.splice(this.prefixes.indexOf(prefix), 1);
      this.generate();
    },
    addSufix(sufix) {
      this.sufixes.push(sufix);
      this.sufix = "";
      this.generate();
    },
    deleteSufix(sufix) {
      this.sufixes.splice(this.sufixes.indexOf(sufix), 1);
      this.generate();
    },
    generate() {
      this.domains = [];
      for (const prefix of this.prefixes) {
        for (const sufix of this.sufixes) {
          this.domains.push(prefix + sufix);
        }
      }
    }
  }
};
</script>

<style>
.slogan {
  margin-top: 30px;
  margin-bottom: 30px;
}

.main {
  background-color: #f1f1f1;
  padding-top: 30px;
  padding-bottom: 30px;
}

</style>

<!-- 
https://www.youtube.com/watch?v=zhbOh6zFCuc&list=PLQCmSnNFVYnTiC-pPY0SySbf-ZNGBwnaG&index=2

tem que ver porque quando clica no delete ele adiciona mais na lista... affs

-->
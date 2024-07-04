<template>
  <v-menu offset-y>
    <template v-slot:activator="{ on }">
      <v-btn color="primary" dark v-on="on">
        {{ selectedFactor ? selectedFactor.factorType : "Facteur d'émission" }}
      </v-btn>
    </template>
    <v-list>
      <v-list-item
        v-for="(value, key) in factors"
        :key="key"
        @click="selectFactor(key, value)"
        :class="{ 'v-list-item--active': key === selectedFactor?.factorType }"
      >
        <v-list-item-title>{{ key }}</v-list-item-title>
        <v-list-item-subtitle>{{ value }}</v-list-item-subtitle>
      </v-list-item>
    </v-list>
  </v-menu>
</template>

<script lang="ts">
import axios from "axios";
import { defineComponent } from "vue";

interface Factors {
  [key: string]: number;
}

interface Factor {
  factorType: string;
  value: number;
}

export default defineComponent({
  name: "FactorsMenu",
  data() {
    return {
      factors: {} as Factors,
      selectedFactor: null as Factor | null,
    };
  },
  created() {
    axios
      .get<Factors>("http://127.0.0.1:8000/")
      .then((response) => {
        this.factors = response.data;
      })
      .catch((error) => {
        console.error("Erreur Axios:", error);
      });
  },
  methods: {
    selectFactor(factorType: string, value: number) {
      this.selectedFactor = { factorType, value };
      this.$emit("FactorSelected", this.selectedFactor);
    },
  },
});
</script>

<style scoped>
.v-list-item--active {
  background-color: #f0f0f0;
}
</style>

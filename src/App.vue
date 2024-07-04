<template>
  <v-app>
    <v-main>
      <v-container fluid>
        <EmissionsMenu @FactorSelected="updateCalculation" />
        <v-row align="center">
          <v-col cols="12" sm="6" md="4">
            <input
              type="number"
              v-model.number="userInput"
              placeholder="1234"
              class="custom-input"
              @input="calculateResult"
            />
            <span>kWh</span>
          </v-col>
          <v-col cols="12">
            <p v-if="calculationResult && userInput">
              Le résultat de {{ selectedFactor.value }} kg CO2/kWh *
              {{ userInput }} kWh est : {{ calculationResult }} kg CO2 pour du
              {{ selectedFactor.factorType }}
            </p>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import EmissionsMenu from "./components/FactorsMenu.vue";

interface Factor {
  factorType: string;
  value: number;
}

export default defineComponent({
  name: "App",
  components: {
    EmissionsMenu,
  },
  data() {
    return {
      userInput: null as number | null,
      selectedFactor: null as Factor | null,
      calculationResult: null as string | null,
    };
  },
  methods: {
    updateCalculation(factor: Factor) {
      this.selectedFactor = factor;
      this.calculateResult();
    },
    calculateResult() {
      if (this.selectedFactor && this.userInput) {
        this.calculationResult = (
          this.userInput * this.selectedFactor.value
        ).toFixed(1);
      } else {
        this.calculationResult = null;
      }
    },
  },
});
</script>

<style>
.custom-input {
  border: 1px solid #ccc;
  margin-top: 10px;
  margin-right: 5px;
  padding: 8px;
  width: 100px;
}
</style>

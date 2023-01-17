<template>
  <div>
    <h2>Forex Table</h2>
    <h4>{{ data.date }}</h4>
    <h4>({{ data.base }})</h4>
    <div style="display: flex">
      <table style="width: 50%">
        <tr>
          <th>Currency</th>
          <th>Rates</th>
          <th>Rounded Rates</th>
        </tr>
        <tr v-for="(data, index) in rates" :key="`currency${index}`">
          <td
            :style="{
              'border-color':
                isEvenNumber(data.rate) || isHKD(data.currency) ? 'red' : '',
            }"
          >
            {{ data.currency }}
          </td>
          <td
            :style="{
              'border-color':
                isEvenNumber(data.rate) || isHKD(data.currency) ? 'red' : '',
            }"
          >
            {{ data.rate }}
          </td>
          <td
            :style="{
              'border-color':
                isEvenNumber(data.rate) || isHKD(data.currency) ? 'red' : '',
            }"
          >
            {{ Math.round(data.rate) }}
          </td>
        </tr>
      </table>
      <table style="width: 50%">
        <tr>
          <th>Currency</th>
          <th>Modified Rates</th>
          <th>Rounded Rates</th>
        </tr>
        <tr
          v-for="(data, index) in modifiedRates"
          :key="`modifiedCurrency${index}`"
        >
          <td
            :style="{
              'border-color':
                isEvenNumber(data.rate) || isHKD(data.currency)
                  ? 'red'
                  : 'black',
            }"
          >
            {{ data.currency }}
          </td>
          <td
            :style="{
              'border-color':
                isEvenNumber(data.rate) || isHKD(data.currency)
                  ? 'red'
                  : 'black',
            }"
          >
            {{ data.rate }}
          </td>
          <td
            :style="{
              'border-color':
                isEvenNumber(data.rate) || isHKD(data.currency)
                  ? 'red'
                  : 'black',
            }"
          >
            {{ Math.round(data.rate) }}
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "../axios";
import { get } from "lodash";

export default {
  name: "ForexTable",
  data() {
    return {
      data: {},
      rates: [],
      modifiedRates: [],
    };
  },
  async mounted() {
    await this.getForexData();
  },
  methods: {
    get,
    async getForexData() {
      try {
        this.data = (await axios.get("/getForexData")).data;
      } catch (e) {
        console.log(e);
        window.alert(`${e.response.data.info}`);
        return;
      }
      this.rates = Object.entries(this.data.rates).map((rate, index) => {
        return { currency: rate[0], rate: rate[1] };
      });
      this.modifiedRates = this.rates.map((x) => {
        const newRate = x.rate + 10.0002;
        return {
          currency: x.currency,
          rate: newRate,
        };
      });
      console.log(this.modifiedRates);
    },
    isEvenNumber(rate) {
      const roundedRate = Math.round(rate);
      return roundedRate % 2 === 0;
    },
    isHKD(currency) {
      return String(currency).toLowerCase() === "hkd";
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table,
th,
td {
  border: 1px solid black;
}
</style>

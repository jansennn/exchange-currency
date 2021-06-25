<template>
  <div id="app">
    <b-container fluid="md" class="mt-3">
      <div class="col-md-12 shadow p-3 mb-5 bg-white rounded">
        <div class="row">
          <div class="col-md-6">
          <h2>EUR</h2>
        </div>
        <div class="col-md-6">
          <b-form-input v-model="base" ></b-form-input>
        </div>
        </div>
      </div>

      <div class="col-md-12 shadow p-3 mb-5 bg-white rounded" v-for="(price, name, index) in items" :key="index">
        <div class="row">
          <div class="col-md-11">
            <b-row>
              <b-col sm="8"><h4>{{ name }}</h4></b-col>
              <b-col sm="4"><h4>{{ base*price }}</h4></b-col>
            </b-row>
            <b-row>
              <b-col sm="SGD - Singapore Dollar"></b-col>
              <b-col sm="12">1 EUR = {{ name }} {{ price }}</b-col>
            </b-row>
          </div>
          <div class="col-md-1">
            <button v-on:click="deleteCurrency(index)"> - </button>
          </div>
        </div>
      </div> 

      <div class="col-md-12 shadow p-3 mb-5 bg-white rounded">
        <div class="row">
          <div class="col-md-9">
            <b-form-select v-model="selected" :options="options" size="sm" class="mt-3"></b-form-select>
          </div>
          <div class="col-md-3">
            <b-button variant="outline-primary" class="align-middle mt-2" v-on:click="addCurrency">Submit</b-button>
          </div>
        </div>
      </div>

    </b-container>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {

  },
  data(){
    return {
      base: 10,
      items: [],
      options: [
        { value: "USD", text: 'USD' },
        { value: "CAD", text: 'CAD' },
        { value: "IDR", text: 'IDR' },
        { value: "GBP", text: 'GBP' },
        { value: "CHF", text: 'CHF' },
        { value: "SGD", text: 'SGD' },
        { value: "INR", text: 'INR' },
        { value: "MYR", text: 'MYR' },
        { value: "JPY", text: 'JPY' },
        { value: "KRW", text: 'KRW' },
      ],
      currencys : ["IDR","GBP","SGD","USD"],
      selected : ""
    }
  },
  methods: {
    setCurrency(data){
      this.items = data
    },
    addCurrency(){
      this.currencys.push(this.selected);
      console.log(this.currencys);
      this.loadCurrency();
    },
    loadCurrency(){
      axios
      .get(`http://api.exchangeratesapi.io/v1/latest?access_key=ade54a988e6bf699a2f95420711e12fb&symbols=${this.currencys}`)
      .then((response) => this.setCurrency(response.data.rates))
      .catch(function (error) {
        console.log("Gagal : ", error);
      });
    },
    deleteCurrency(data){
      this.currencys.splice(data, 1);
      console.log(this.currencys);
      console.log(data);
      this.loadCurrency();
    }
  },
  mounted(){
    axios
      .get(`http://api.exchangeratesapi.io/v1/latest?access_key=ade54a988e6bf699a2f95420711e12fb&symbols=${this.currencys}`)
      .then((response) => this.setCurrency(response.data.rates))
      .catch(function (error) {
        console.log("Gagal : ", error);
      });
 
  }
}
</script>

<style>

</style>

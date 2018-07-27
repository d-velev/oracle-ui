<template>
  <div id="oracle-query">
    <form id="oracle-query-form" class="oracle-query-form" @submit.prevent="onSubmit">
      <ae-label>Oracle address</ae-label>
      <ae-input id="oracle-id" placeholder="oracle hash" v-model="hash"/>
      <ae-label>Query</ae-label>
      <ae-input placeholder="query data" v-model="query"/>
      <ae-label>Query TTL</ae-label>
      <ae-input placeholder="query TTL" v-model="queryTTL"/>
      <ae-label>Response TTL</ae-label>
      <ae-input placeholder="response TTL" v-model="responseTTL"/>
      <ae-label>Query Fee</ae-label>
      <ae-input type="number" v-model="queryFee"/>
      <ae-label>Fee</ae-label>
      <ae-input type="number" v-model="fee"/>
    </form>
    <ae-button block
               form="oracle-query-form"
               :type="hash && query && queryFee && fee? 'dramatic': 'boring'"
               :inactive="!hash || !query || !queryFee || !fee">
      Send Query
    </ae-button>
  </div>
</template>

<script>
  import axios from 'axios'
  import {AeLabel, AeButton, AeInput} from '@aeternity/aepp-components'

  export default {
    name: 'OracleQuery',
    components: {
      AeLabel, AeButton, AeInput
    },
    data () {
      return {
        hash: '',
        queryFee: 0,
        fee: 0,
        query: '',
        queryTTL: '',
        responseTTL: ''
      }
    },
    methods: {
     onSubmit () {
       let data = {
         hash: this.hash,
         query_fee: parseInt(this.queryFee, 10),
         fee: parseInt(this.fee, 10),
         query: this.query,
         query_ttl: this.queryTTL,
         response_ttl: this.responseTTL
       }
       axios.post(`http://localhost:4000/oracle_query`, data)
     }
   }
  }
</script>

<style>
  #oracle-query {
    display: flex;
    flex-direction: column;
    border: 2px solid #f03c6e;
    margin: 8px;
    padding: 16px;
  }
</style>

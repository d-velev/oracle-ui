<template>
  <div id="app">
    <ae-main id="main">
      <registered-oracles></registered-oracles>
      <oracle-query></oracle-query>
    </ae-main>
  </div>
</template>

<script>
import RegisteredOracles from './components/RegisteredOracles'
import OracleQuery from './components/OracleQuery.vue'
import {AeMain} from '@aeternity/aepp-components'

export default {
  name: 'App',
  components: {
    RegisteredOracles,
    OracleQuery,
    AeMain
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: left;
    color: #2c3e50;
    margin-top: 60px;
  }
  #main{
    display: flex;
    flex-direction: row;
  }
</style>

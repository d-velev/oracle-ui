<template>
  <div id="oracles">
    <ae-label id="ae-label">
      <strong>Available oracles</strong>
    </ae-label>
    <div v-for="oracle in oracles">
      <ae-panel :ratioTop="1" :ratioBottom="1">
        <div id="hash">
          <strong> Address: </strong>{{oracle.body.data.sender}}
        </div>
        <ae-divider/>
        <div>
          <strong> Query format: </strong>{{oracle.body.data.payload.query_format}}
        </div>
        <ae-divider/>
        <div>
          <strong> Response format: </strong>{{oracle.body.data.payload.response_format}}
        </div>
      </ae-panel>
    </div>
  </div>
</template>

<script>
  import {AeLabel, AePanel, AeDivider} from '@aeternity/aepp-components'

  let phoenix = require('phoenix-js')
  let axios = require('axios');
  axios.defaults.timeout = 20000;

  export default {
    name: 'RegisteredOracles',
    components: {
      AeLabel, AePanel, AeDivider
    },
    data () {
      return {
        oracles: []
      }
    },
    mounted () {
      let socket = new phoenix.Socket('ws://localhost:4000/socket')
      let channel = socket.channel('room:notifications')



      socket.connect()

      channel.on('new_mined_oracle_register_tx:ak$6JADPYK3cR4j55qHkGoXz99TgmiALXFTLbDDmNoaFDNDPFSgTw', msg => this.oracles.push(msg))
      channel.on('new_mined_oracle_query_tx:ak$6JADPYK3cR4j55qHkGoXz99TgmiALXFTLbDDmNoaFDNDPFSgTw', msg =>
        axios.get('http://localhost:4000/temperature/' + msg.body.data.payload.query_data.city).then(function (response) {
               axios.post(`http://localhost:4000/oracle_response`, {"query_id": msg.id, "fee": 10, "response": response})}
         ))

      channel.join()
      .receive('ok', ({messages}) => console.log('Successful join', messages))
      .receive('error', ({reason}) => console.log('Failed join', reason))
      .receive('timeout', () => console.log('Timeout when joining'))
    }
  }
</script>

<style>
  #oracles {
    border: 2px solid #f03c6e;
    padding: 16px;
    margin: 8px;
    width: 880px;
    max-height: 856.6px;
    overflow-y: auto;
    overflow-x: inherit;
  }
  #ae-label {
    text-align: center
  }
  #hash {
    word-wrap: break-word
  }
</style>

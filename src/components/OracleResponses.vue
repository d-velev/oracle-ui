<template>
  <div id="responses">
    <ae-label id="ae-label">
      <strong>Oracle responses</strong>
    </ae-label>
    <div v-for="response in responses">
      <ae-panel :ratioTop="1" :ratioBottom="1">
        <div id="hash">
          <strong> Oracle address: </strong>{{response.body.data.sender}}
        </div>
        <ae-divider/>
        <div>
          <strong> Query ID: </strong>{{response.body.data.payload.query_id}}
        </div>
        <ae-divider/>
        <div>
          <strong> Response: </strong>{{response.body.data.payload.response}}
        </div>
      </ae-panel>
    </div>
  </div>
</template>

<script>
  import {AeLabel, AePanel, AeDivider} from '@aeternity/aepp-components'

  let phoenix = require('phoenix-js')

  export default {
    name: 'OracleResponses',
    components: {
      AeLabel, AePanel, AeDivider
    },
    data () {
      return {
        responses: []
      }
    },
    mounted () {
      let socket = new phoenix.Socket('ws://localhost:4000/socket')
      let channel = socket.channel('room:notifications')

      socket.connect()

      channel.on('new_mined_oracle_response_tx:ak$8AE9di2BBRNSUi1k7FQhKP3BvGSbb7ggnvqZFpqd9Fm6jxhsWn', msg => this.responses.push(msg))

      channel.join()
      .receive('ok', ({messages}) => console.log('Successful join', messages))
      .receive('error', ({reason}) => console.log('Failed join', reason))
      .receive('timeout', () => console.log('Timeout when joining'))
    }
  }
</script>

<style>
  #responses {
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

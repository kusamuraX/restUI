<template>
  <div class="container">
    <div class="request_area">
      <label>Method</label>
      <b-form-select v-model="selected" id="method_select">
        <option v-for="name in methods" :key="name">{{ name }}</option>
      </b-form-select>
      <label >Url</label>
      <b-form-input v-model="rest_url" type="text" />
      <b-button  variant="primary" v-on:click="send">SEND</b-button>
    </div>
    <div class="request_body">
      <label>Request Body</label>
      <b-form-textarea id="textarea1"
                          v-model="request_body"
                          placeholder="Enter request body"
                          :rows="3"
                          :max-rows="6">
      </b-form-textarea>
    </div>
    <div class="response_body">
      <label>Response
        <span v-if="!isNaN(status_code) && status_code >= 200 && status_code <= 299"><b-badge variant="primary">{{ status_code }}</b-badge></span>
        <span v-else><b-badge variant="danger">{{ status_code }}</b-badge></span>
      </label>
      <br>
      <div>
        <b-tabs content-class="mt-3">
          <b-tab title="Body" active>
            <b-form-textarea id="textarea1"
                                v-model="response_data"
                                placeholder="Enter something"
                                :rows="6"
                                :max-rows="10">
            </b-form-textarea>
          </b-tab>
          <b-tab title="Header">
            <b-form-textarea id="textarea1"
                                v-model="response_header"
                                placeholder="Enter something"
                                :rows="6"
                                :max-rows="10">
            </b-form-textarea>
          </b-tab>
        </b-tabs>
      </div>
    </div>
  </div>
</template>
<script>
  import axios from 'axios'

  export default {
    name: 'rest-page',
    data () {
      return {
        selected: 'GET',
        methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS', 'HEAD', 'TRACE', 'CONNECT', 'PATCH'],
        request_body: null,
        response_data: null,
        response_header: null,
        status_code: null,
        rest_url: 'http://localhost:8080/test'
      }
    },
    methods: {
      send: function () {
        axios({
          method: this.selected,
          url: this.rest_url,
          headers: {}
        })
          .then(response => {
            this.status_code = response.status
            this.response_data = JSON.stringify(response.data, undefined, '\t')
          })
          .catch(error => {
            this.status_code = error.response
            console.log(error)
            this.response_data = error.message
          })
      }
    }
  }
</script>

<style>
  body{
    font-family: 'Source Sans Pro', sans-serif;
    background-color: #616161;
    font-size: 1.2em;
    color: #FFF;
  }

  .request_area {
    margin-top: 1em;
    display: -webkit-flex;
    display: flex;
  }

  .request_body {
    margin-top: 2em;
  }

  .response_body {
    margin-top: 2em;
  }

  #method_select{
    width: 8em;
  }
</style>

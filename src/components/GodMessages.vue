<template>
  <div>
    <div class='ui center aligned basic segment history-bz'>
      <old :god_name="god_name"></old>
    </div>
    <message v-for="message in messages" :message='message'></message>
    <div class='ui active centered inline loader' v-bind:class="{ 'invisible_bz': !new_loading}"></div>
    <bottom-loader :el="$el" element_class=".ui.fluid.card" v-on:bottom="call_back"></bottom-loader>
  </div>
</template>

<script>
  import Old from './Old'
  import Message from './Message'
  import BottomLoader from 'bz-bottom-loader'

  module.exports = {
    components: {
      Old,
      Message,
      BottomLoader
    },
    props: {
      god_name: {
        type: String,
        required: true
      }
    },
    data: function () {
      return {
        filter_pic: false,
        error_info: '',
        last_message: null
      }
    },
    computed: {
      new_loading () {
        return this.$store.state.new_loading
      },
      messages () {
        return this.$store.state.gods_messages[this.god_name]
      }
    },
    mounted () {
      this.$store.commit('FILTER_GOD_MESSAGES', this.god_name)
      this.$store.dispatch('newMessage', {god_name: this.god_name})
    },
    methods: {
      call_back: function () {
        this.$store.dispatch('newMessage', {god_name: this.god_name})
      }
    }
  }
</script>

<style >
.invisible_bz {
  visibility:hidden;
};
</style>

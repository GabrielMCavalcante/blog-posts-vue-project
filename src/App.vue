<template>
  <div>
      <div v-bind:style="'margin-bottom:' + offset">
        <Header></Header>
      </div> 
      <router-view v-on:changeOffset="changeOffset"></router-view>
  </div>
</template>

<script>
import Header from './components/Header.vue';

export default {
  components: {
    Header
  },
  data() {
    return {
        offset: new String()
    }
  },
  methods: {
    changeOffset(offset) {
      this.offset = offset;
    }
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
    font-family: 'Fira Code', sans-serif;
  }
  body {
    margin: 0;
    padding: 0;
    background: #181A1B;
    color: #E8E6E3;
  }
  input[type="text"], 
  textarea,
  select {
      display: block;
      width: 100%;
      padding: 8px;
      outline: none;
      border: 1px solid rgba(255,255,255,.1);
      border-radius: 8px;
      background: #181A1B;
      color: #E8E6E3;
  }
  input:focus,
  textarea:focus,
  select:focus {
    border: 1px solid white;
  }
  button {
    padding: 8px;
    font-size: 13px;
    outline: none;
    border: 1px solid rgb(102, 102, 102);
    border-radius: 8px;
    cursor: pointer;
    transition: 200ms all;
    background-color: #181A1B;
    color: #E8E6E3;
  }
  button:hover {
    filter: brightness(80%);
  }
</style>
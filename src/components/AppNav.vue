<template>
  <b-card class="nav" color="grey lighten-4" flat height="200px" tile>
    <b-nav>
      <h5>
        <a id="python" href="https://valdrinkuchi.com">HOME</a>
      </h5>
    </b-nav>
  </b-card>
</template>

<script>
// import { authComputed } from '../vuex/helpers.js'
export default {
  computed: {
    // ...authComputed
  },
  methods: {}
};
</script>

<style lang="scss" scoped>
h5,
#python {
  width: 100%;
  margin: 0 auto;
  font-family: helvetica;
  text-align: center;
  color: #09263d;
  font-weight: bold;
  margin-top: 5px;
  font-size: 20px;
  letter-spacing: 8px;
}
</style>

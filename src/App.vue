<template>
  <div id="app">
    <app-nav />
    <div class="container">
      <div class="shadow p-3 mb-5 rounded">
        <div id="title" class="shadow p-3 mb-5 rounded">
          <h5>MY PORTFOLIO</h5>
        </div>
        <div class="shadow p-3 mb-5 rounded">
          <AppForm />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppNav from "./components/AppNav";
import AppForm from "./components/AppForm";

export default {
  name: "App",

  components: {
    AppNav,
    AppForm
  },

  data: () => ({
    //
  })
};
</script>
<style scoped>
.container {
  margin-top: 50px;
}
#app {
  /* padding-left: calc(100vw - 100%); */
  overflow-y: scroll;
  overflow-x: hidden;
  margin-right: calc(-1 * (100vw - 100%));
}
#title {
  width: 50%;
  max-width: 50% !important;
  min-width: 250px !important;
  margin: 0 auto;
  border-radius: 50px !important;
}
h5 {
  width: 100%;
  margin: 0 auto;
  font-family: helvetica;
  text-align: center;
  color: #09263d;
  font-weight: bold;
  margin-top: 5px;
  font-size: 18px;
  letter-spacing: 12px;
}
@media (max-width: 900px) {
  h5 {
    width: 100%;
    margin: 0 auto;
    font-family: helvetica;
    text-align: center;
    color: #09263d;
    font-weight: bold;
    margin-top: 5px;
    font-size: 15px;
    letter-spacing: 12px;
  }
}
</style>

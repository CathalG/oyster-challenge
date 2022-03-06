<template>
  <div class="header-container">
    <img alt="Oyster logo" class="logo" src="./assets/logo.svg">
    <h2>Tax Calculator</h2>
  </div>
  <div class="page-container-outer">
    <div id="page-container-inner">
        <TaxCalculator/>
    </div>
  </div>
</template>

<script>
import TaxCalculator from './components/TaxCalculator.vue'

export default {
  name: 'App',
  components: {
    TaxCalculator
  },
  data () {
    return {
    }
  }

}
</script>

<style>
body{
  background-size:cover ;
  background-repeat: no-repeat;
  min-height: 100vh;
  padding-bottom: 50px;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
  flex: 1;
  display: flex;
  flex-direction: column;
  background-image: url("./assets/background.png");
  background-size:cover ;
  background-position:center ;
  border-radius: 50px;
  margin: 40px;
  border-radius: 10px;
  margin: 30px;
  position: relative;
}

.logo{
  width: 130px;;
}

.header-container {
    display: flex;
    padding: 15px 30px;
    justify-content: flex-start;
    position: absolute;
}

.header-container h2{
  margin-left: 20px;
}

.page-container-outer {
    max-width: none;
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
}

</style>

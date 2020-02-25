<template>
  <div class="hello">
   <div class="world">
    <h1>{{ msg }}</h1>
    <section v-if="errored">
    <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
  </section>
   <div v-if="loading">Loading...</div>

    <p v-for="parabirimi in info" :key="parabirimi.id">
      {{parabirimi.description}} : <strong> {{parabirimi.rate_float | fazlayiat}} <span v-html="parabirimi.symbol"></span></strong>
    </p>
   </div>
  </div>
</template>

<script>
import axios from "axios"
export default {
  data() {
    return {
      info : null,
      loading : true,
      errored : false,
    
    }
  },
  name: 'HelloWorld',
  props: {
    msg: String
  },

  mounted (){
    axios
    .get("https://api.coindesk.com/v1/bpi/currentprice/TRY.json")
    .then(response => {this.info = response.data.bpi})
    .catch(error => {
      console.log(error)
      this.errored = true;
    })
    .finally(() => this.loading = false)
  },


  filters: {
    fazlayiat (value) {
      return value.toFixed(1)
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h2 {
  margin: 20px 0 0;
}
.hello {
  background-color: blanchedalmond;
  display: flex;
  justify-content: center;
  height: 100vh;
}
.world {
  width: 400px;
  background-color:#2c3e50;
  height: auto;
}
.world p {
  border: 1px solid #fefefe;
  padding: 10px 30px 10px 30px;
  margin: 30px;

}
</style>

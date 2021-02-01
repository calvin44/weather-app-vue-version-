<template>
<div class="my-container">
    <div>
        <h1 class="text-center app-name">Weather App</h1>
        <p class="error-message">{{err}}</p>
        <form v-on:submit.prevent="search">
          <div class="input-group mb-3 me-auto ms-auto input">
            <input type="text" v-model="input"  class="form-control text-center" placeholder="Enter city name" />
            </div>
        </form>
        <div v-if="fetched">
         <div class="card border-secondary mb-3 text-center mx-auto" style="max-width: 18rem;">
          <div class="card-header">
            <h4>Weather Information</h4>
          </div>
          <div class="card-body">
            <img v-bind:src= "'http://openweathermap.org/img/wn/'+data.data.weather[0].icon+'@2x.png'" alt="">
            <h5 class="card-title">{{city | capitalize}}</h5>
            <p class="card-text mb-2 mt-3">{{data.data.weather[0].description | capitalize}}</p>
            <p class="card-text">{{data.data.main.temp | celcius}} °C</p>
          </div>
        </div>
      </div> 
      <Footer />
    </div>
</div>
</template>

<script>
import axios from "axios";
import Footer from "./components/footer"


export default {
  name: "App",
  components: {
    Footer
  },
  data() {
    return {
      city: "",
      input: "",
      fetched:false,
      data: null,
      err: null,
    };
  },
  filters: {
    capitalize: function(a) {
      let word  = a.split(" ");
      for(let i = 0; i < word.length; i++){
        word[i] = word[i][0].toUpperCase() + word[i].slice(1)
      }
      return word.join(" ");
    },


    celcius: function(e) {
    return (e - 273.15).toFixed(2);
    }
  },
  methods: {
    search: function () {
      axios
        .get(`http://api.openweathermap.org/data/2.5/weather?q=${this.input}&appid=bae2183dad4c9ecea641d6e6a899efcb`)
          .then(data => {
            this.data = data;
            this.fetched = true;
            this.err = null;
            this.city = this.input;
            this.input="";
            console.log(this.input)
          })
          .catch(() => {
            this.err = "please enter city name!!"
            this.data = ""
            this.city = ""
            this.input = ""
            // console.log(this.input)
          })
    },
  },
};
</script>



<style>
@import url('https://fonts.googleapis.com/css2?family=Ubuntu:wght@300&display=swap');

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  color: #2c3e50;
}

.app-name {
  font-weight: bold;
}

.search-button {
  cursor: pointer;
}

.error-message {
  color: red;
  text-align: center;
}

.my-container {
  font-family: 'Ubuntu', sans-serif;
  position : absolute;
  height: 100%;
  width:100%;
  display: flex;
  justify-content: center;
  align-items:center;
}
</style>

<template>
  <div class="hello">
    <header>
      <div id="covid-tracker-header">
        <h1>{{ msg }}
          <!-- <img src= "../assets/COVID-19 icon.png"> -->
        </h1>
      </div> 
    </header>
    <div id="covid-data-distribution">
      <h2>Total Active</h2>
      <h2>Total Confrimed</h2>
      <h2>Total Deaths</h2>
    </div>
    <div id="covid-data-distribution">
      <h3>{{this.globaldata.active}}</h3>
      <h3>{{this.globaldata.confirmed}}</h3>
      <h3>{{this.globaldata.deaths}}</h3>
    </div>
    <h3>Select the state to view details</h3>
    <ul class="list-rows">
      <select v-model="stateName" v-on:change="getStateWiseDate()" id="state-dropdown">
      <option v-for="state in this.states" :key="state">
        {{state}}
      </option>
      </select>
    </ul>
    <div id="covid-data-distribution" v-if="stateName">
      <h3>{{this.statewiseData.active}}</h3>
      <h3>{{this.statewiseData.confirmed}}</h3>
      <h3>{{this.statewiseData.deaths}}</h3>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      states: [],
      globaldata: {
        active: 383878,
        confirmed: 29899898,
        deaths: 8989
      },
      stateName: "",
      statewiseData: {
        active: '',
        confirmed: '',
        deaths: ''
      }
    }
  },
  mounted () {
    axios
      .get('http://localhost:8080/covid/getallstates')
      .then(response => {
        this.states = response.data.allStates
        }),    
    axios
      .get('http://localhost:8080/covid/globaldata')
      .then(response => {
        this.globaldata.active=response.data.active
        this.globaldata.confirmed = response.data.confirmed
        this.globaldata.deaths = response.data.deaths

        console.log(this.globaldata)
        })
  },

  methods:{
    getStateWiseDate: function () {
      console.log(this.stateName)
      axios
      .get('http://localhost:8080/covid/state/'+ this.stateName)
      .then(response => {
        console.log(response)
        this.statewiseData.active= response.data.active
        this.statewiseData.confirmed = response.data.confirmed
        this.statewiseData.deaths = response.data.deaths
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

/* header styling - start */
#covid-tracker-header{
   -ms-flex: 1;  /* IE 10 */  
  flex: 1;
  height: 100px;
  padding: 1px;
  background-color: #e9eff0;
  color: navy;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-size: larger;

}


/* header styling - end */

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

#covid-data-distribution {
  display: flex;
} 

#covid-data-distribution h2 {
  -ms-flex: 1;  /* IE 10 */  
  flex: 1;
  margin: 10px;
  color: brown;

}

#covid-data-distribution h3 {
  -ms-flex: 1;  /* IE 10 */  
   margin: 10px;
  flex: 1;
}

#state-dropdown{
  background: #9269e238;
  padding: 10px;
  width: 300px;
  border-radius: 6px;
}

img{
      margin-bottom: 60px;
    opacity: 0.3;

}
</style>

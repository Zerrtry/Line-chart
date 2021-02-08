<template>
    <v-container> 
      <v-row>
        <v-col>
          <div>
            <p style="margin-botton: 10px; padding: 20px">The number of nodes at most 20:</p>
            <v-slider
              v-model="slider"
              thumb-label="always"
              :thumb-size="24"
              max="20"
              min="2"
              color="#42b983"
            ></v-slider>
          </div>
          <form id='form' @submit.prevent="displayChart">
              <p style="padding: 20px;">Please define values between 0 and 100:</p>
              <ul class='row'>
              <li class='col-6' v-for='i in slider'
               v-bind:key='i'>
               <!-- <div> -->
              <label 
                :for='i' 
                style="text-align: right; clear: both; float:left; ">
                <span v-if="i === 1">Start :</span>
                <span v-else> Node {{i-1}}: </span>
              </label>
              <span style="display:block">
              <input 
                v-on:input='myChangeFunction($event.target.value,$event.target.id)'
                :id='i' 
                type='number'
                min=0 
                max=100 
                placeholder='0 to 100'
                name='nodeValue'
              />
              </span>
                <!-- </div> -->
              </li>
            </ul>
            <button style="background-color: #42b983; padding: 5px 10px; border-radius: 4px; border: 2px solid black; margin: 20px">Display Chart</button>
          </form>
        </v-col>
  <v-col>
    <highcharts :options="chartOptions"></highcharts>
  </v-col>
      </v-row>  
    </v-container>
</template>

<script>
import {Chart} from 'highcharts-vue'


export default {
  name:"Graph",
  components:{
    highcharts: Chart
  },
  data () {
    return {
      slider:10,
      nodesVelues:[],
      chartOptions: {
        chart: {
          type: 'spline'
        },
        colors:['#42b983'],
        xAxis:{
          title: {
            text: "Nodes"
          },
        },
        yAxis: {
          title: {
            text: "Values",
          },
          gridLineWidth: 1,
          max:100,
        },
        series: [
        ],
        title: {
          text: 'This chart displays user`s input data',
        },
      },
    }
  },

  methods: {
    displayChart: function (event) {
      var data = Array.from(this.nodesVelues, item => item || null)
      this.chartOptions.series.push({
        data: data
      })
      this.nodeValue=''
      event.target.reset()
    },
    myChangeFunction(a,b){
      if (a<=100) {
        var item = b-1
        this.nodesVelues[item] = parseInt(a) 
      }
    },
  },
}
</script>

<style>
  input[type=number] {
    padding: 5px;
    margin: 0 10px;
    box-sizing: border-box;
    border: 1px solid black;
    border-radius: 3px;
  }
  ul {
    list-style-type:none
  }
  button {
    background-color: #42b983;
  }
</style>
<template>
  <div class="py-8 px-8 md:px-0 bg-green-900">
    <div class="container mx-auto flex flex-col items-center">
      <h2 class="text-2xl leading-none font-bold text-center pb-8 text-white">
        First {{numberOfDays }} days of the COVID-19
      </h2>

      <div class="controls-wrapper flex items-center mb-8">
        <div class="text-white uppercase text-sm  mr-2">Choose a number</div>
        <input class="h-10 w-20 text-center " type="number" placeholder="Number of days" v-model="numberOfDays">
<!--        <button v-if="!getCuratedTimeseries.labels.length < 2" @click="FETCH_TIMESERIES" class="h-10 mx-auto btn p-2 bg-green-600 hover:bg-green-700 text-xs uppercase font-bold rounded text-white inline-block mb-8">Press to load data</button>-->
<!--        <div v-else>Data loaded succesfully</div>-->
      </div>


      <div class="chart w-full">
        <line-chart
          :responsive="true"
          :chart-data="datacollection"
          :options="options"
        ></line-chart>
      </div>
      <div>
        <p class="text-gray-500 text-center pt-5">
          The first {{ numberOfDays }} days of the virus in these countries.
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import LineChart from "../helpers/LineChart";
import BarChart from "../helpers/BarChart";
import Datepicker from 'vuejs-datepicker';
import { trimEmptyCountryData, trimEmptyCountriesData, random_rgba, pickColor } from '../helpers'

import { mapGetters, mapActions } from "vuex";
export default {
  components: {
    LineChart,
    BarChart,
    Datepicker
  },
  data() {
    return {
      datacollection: null,
      startDate: new Date('03/01/2020'),
      endDate: new Date(),
      dayInterval : 1000 * 60 * 60 * 24,
      numberOfDays: 10,
      options: {
        responsive: true,
        maintainAspectRatio: false,
        legend: {
          display: true,
          labels: {
            fontColor: "rgb(255, 255, 255)"
          }
        },
        scales: {
          yAxes: [
            {
              gridLines: {
                color: "rgba(255,255,255,0.1)"
              },
              ticks: {
                stepSize: 10,
              }
            }
          ],
          xAxes: [
            {
              bounds: 'ticks',
              ticks: {
                source: 'labels'
              }

            }
          ]
        }
      },
      points: {
        pointStyle: "circle",
        borderWidth: 1,
        datasetStrokeWidth: 1
      }
    };
  },
  mounted() {
    this.FETCH_TIMESERIES();
  },
  methods: {
    ...mapActions(['FETCH_TIMESERIES']),
    fillData() {
      let dataset = []
      console.log(this.getCuratedTimeseries)
      for (let country in this.getCuratedTimeseries.simple){
        let highlightedCountryConfig = {
          borderColor: pickColor(country)
        }

        if (country === 'Mauritius') {
          highlightedCountryConfig = {
            borderWidth: 4,
            borderColor: "#E44450",
          }
        }

        console.log(country)
        console.log(this.getCuratedTimeseries.simple[country])
        console.log(trimEmptyCountryData(this.getCuratedTimeseries.simple[country]).slice(0,this.numberOfDays))

        let countryData = {
              label: country,
              data: trimEmptyCountryData(this.getCuratedTimeseries.simple[country]).slice(0,this.numberOfDays),
              ...this.points,
              ...highlightedCountryConfig
        }
        dataset.push(countryData);
      }

      console.log(this.getCuratedTimeseries.labels)
      let dateRange = this.getDates;
      console.log(dateRange)

      this.datacollection = {
        labels: dateRange,
        datasets: dataset,

      };
    },
  },
  computed: {
    compoundProperty() {
      // watch all these properties
      return this.getCuratedTimeseries, this.numberOfDays, Date.now();
    },
    getDates() {
      let result = [];
      for (let i=0;i < this.numberOfDays;i++) {
        result.push(`Day ${i + 1}`)
      }
      return result
    },
    ...mapGetters([
      "getActive",
      "getTimeseries",
      "getCuratedTimeseries"
    ])
  },
  watch: {
    compoundProperty() {
      this.fillData();
    }
  },
};
</script>

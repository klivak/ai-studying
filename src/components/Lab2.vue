<template>

  <b-card-text class="lab-second">
    <b-alert show class="d-flex justify-content-between">
      <div>Генетичні алгоритми // Оператори: Відбору батьків: рулетка та турнірний
      </div>
    </b-alert>

    <div class="lab2-buttons">
      <b-button @click="initPopulation" class="m-1">
        Ініціалізувати популяцію та обчислити
      </b-button>

      <div>
        Селекція/відбір: <br>
        <div>- Рулетка</div>
        <div>- Турнір</div>
      </div>

      <hr>

      <div v-for="(message, index) of logList" :key="index">
        {{ message }}
      </div>

      <div id="chart" style="max-width: 400px"></div>

    </div>
  </b-card-text>

</template>

<script>
import ApexCharts from 'apexcharts'

export default {
  name: 'LabSecond',
  data() {
    return {
      sizeOfGeneration: 5,

      gensList: null,
      populationList: [],

      logList: []
    }
  },
  methods: {
    initPopulation() {
      // generate list
      for (let i = 0; i <= this.sizeOfGeneration; i++) {
        this.populationList.push({
          x: this.randomInteger(1, 63),
          y: this.randomInteger(0, 63),
          z: this.randomInteger(0, 63),
        })
      }

      // calculate max function
      this.populationList.forEach(item => {
        Object.assign(item, {f: this.calculateFitnessFunction(item.x, item.y, item.z)});
      });

      // calculate percent
      let sum = 0;
      this.populationList.forEach(item => {
        sum += parseFloat(item.f.replace(/\s/g, "").replace(",", "."));
      });

      // sum = 100%
      // value - x%

      this.populationList.forEach(item => {
        let percentValue = item.f * 100 / sum;
        Object.assign(item, {percent: percentValue});
      });

      console.log('sum', sum);
      console.log('populationList', this.populationList);

      // push to log
      this.pushToLog('Population:')
      this.populationList.forEach(item => {
        this.pushToLog(`x: ${item.x}, y: ${item.y}, z: ${item.z}, function result: ${item.f}, percent: ${item.percent}%`);
      })
      this.pushToLog('...')

      // chart
      const series = [];
      const labels = [];
      this.populationList.forEach((item, index) => {
        series.push(item.percent);
        labels.push(`Population: ${index + 1}`);
      });

      let options = {
        chart: {
          type: 'pie'
        },
        series: series,
        labels: labels
      }

      let chart = new ApexCharts(document.querySelector("#chart"), options);

      chart.render();
    },

    calculateFitnessFunction(x, y, z) {
      const value = x + (Math.pow(y, 2) / x) + (Math.pow(z, 2) + y) + (2 / z);

      return (Math.round(value * 100) / 100).toFixed(4);
    },

    randomInteger(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },

    pushToLog(message) {
      this.logList.push(message);
    }
  },
  mounted() {
    // this.initPopulation()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.lab-second {

}

.lab2-buttons {

}
</style>

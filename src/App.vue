<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-12 pt-4 pb-4">
          <b-card no-body>
            <b-tabs pills card vertical class="tabs-list">
              <!-- Lab 1 -->
              <b-tab title="LAB #1" active>
                <b-card-text>
                  <b-alert show class="d-flex justify-content-between">
                    <div>Швидке прибирання столиків у їдальні (25 столів).</div>
                  </b-alert>

                  <div class="buttons-list mb-2">
                    <div>
                      <b-button @click="randomTablePollution" class="m-1">Забруднити столи</b-button>
                      <b-button @click="startAgent" class="m-1" variant="outline-primary">Запустити агента</b-button>
                    </div>

                    <div>
                      <b-button @click="clearAllData" variant="outline" class="m-1">Очистити дані</b-button>
                    </div>
                  </div>

                  <div class="table-description">
                    <div class="table-description__item">
                      <div class="table-description__item-icon table-description__item-green"></div>
                      <div class="table-description__item-text"> Чисті столи (до 5%)</div>
                    </div>

                    <div class="table-description__item">
                      <div class="table-description__item-icon table-description__item-blue"></div>
                      <div class="table-description__item-text"> Трохи забруднені столи (5% - 50%)</div>
                    </div>

                    <div class="table-description__item">
                      <div class="table-description__item-icon table-description__item-orange"></div>
                      <div class="table-description__item-text"> Забруднені столи (50% - 80%)</div>
                    </div>

                    <div class="table-description__item">
                      <div class="table-description__item-icon  table-description__item-red"></div>
                      <div class="table-description__item-text"> Дуже забруднені столи (> 80%)</div>
                    </div>
                  </div>

                  <div class="table-list">
                    <div v-for="(item, id) of tableList" :key="id"
                         class="table-list-item"
                         :class="{
                          'table-list-item-green': item.pollution < 5,
                          'table-list-item-orange': item.pollution > 50,
                          'table-list-item-red': item.pollution > 80
                         }"
                    >
                      <div class="text-center">
                        Стіл №{{ id }}
                      </div>

                      <img src="./assets/table.png" alt="Table">

                      <div class="text-center">
                        Бруд: {{ item.pollution }}%
                      </div>
                    </div>
                  </div>

                  <hr>
                  <h4 v-if="logTree.length">Логування дій</h4>
                  <div ref="loggerList" class="table-logs d-flex flex-wrap-reverse">
                    <div v-for="(message, id) of logTree" :key="id">
                      {{ message }} ->
                    </div>
                  </div>

                </b-card-text>
              </b-tab>

              <!-- Lab 2 -->
              <b-tab title="LAB #2">
                <b-card-text>Lab 2 content</b-card-text>
              </b-tab>
            </b-tabs>
          </b-card>
        </div>
      </div>

    </div>

  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      tableListRequiredAmount: 25,
      tableList: null,
      logTree: [],

      intervalForPollutions: null,
      intervalForCleaning: null
    }
  },
  mounted() {
    this.initTables();
  },
  methods: {
    initTables() {
      const tables = [];
      for (let i = 0; i < this.tableListRequiredAmount; i++) {
        tables.push({
          id: i,
          pollution: 0,
        })
      }
      this.tableList = tables;
    },

    randomTablePollution() {
      this.tableList.forEach(item => {
        item.pollution = Math.floor(Math.random() * 95) + 1;
      });

      this.pushToLogTree('Столи почали забруднюватись');
    },

    startAgent() {

      this.pushToLogTree('Агент запущений');

      this.setIntervalForTablePollution();
      this.setIntervalForCleaning();
    },

    setIntervalForTablePollution() {
      this.intervalForPollutions = setInterval(() => {
        this.pushToLogTree('Cтоли продовжують забруднюватись');

        this.tableList.forEach(item => {
          if (item.pollution < 100) {
            item.pollution = item.pollution + Math.floor(Math.random() * 3) + 1;
          }

          if (item.pollution >= 100) {
            item.pollution = 100;
          }
        });
      }, 1000);
    },

    setIntervalForCleaning() {
      this.intervalForCleaning = setInterval(() => {

        let maxValue = 0;
        let maxValueIndex = null;
        this.tableList.forEach((item, index) => {
          if (item.pollution > maxValue) {
            maxValue = item.pollution;
            maxValueIndex = index;
          }
        });

        this.pushToLogTree(`Прибраний стіл №${maxValueIndex + 1} (було забруднено ${maxValue}%)`)

        this.tableList[maxValueIndex].pollution = 0;

      }, 1000)
    },

    clearAllData() {
      this.initTables();
      this.logTree = [];

      clearInterval(this.intervalForPollutions);
      clearInterval(this.setIntervalForCleaning);
    },

    pushToLogTree(message) {
      this.logTree.push(message);

      // this.$refs.loggerList.offsetTop // TODO: add scroll to top/bottom
    }
  }
}
</script>

<style>
#app {
  min-height: 101vh;
}

.tabs-list {
  min-height: 450px;
}

.tabs-list li {
  margin-bottom: 5px;
}

.table-list {
  display: flex;
  flex-wrap: wrap;
}

.table-list-item {
  width: 90px;
  margin: 10px 5px 30px 5px;
  padding: 10px 12px;
  font-size: 12px;
  background: lightblue;
  border-radius: 30px;
}

.table-list-item-green {
  background: lawngreen;
}

.table-list-item-orange {
  background: coral;
}

.table-list-item-red {
  background: darkred;
}

.buttons-list {
  display: flex;
  justify-content: space-between;
}

.table-description {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.table-description__item {
  display: flex;
  align-items: center;
  margin-right: 20px;
}

.table-description__item-icon {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-right: 5px;
  flex-shrink: 0;
}

.table-description__item-green {
  background: lawngreen;
}

.table-description__item-blue {
  background: lightblue;
}

.table-description__item-orange {
  background: coral;
}

.table-description__item-red {
  background: darkred;
}

.table-list-item img {
  width: 100%;
}

.table-logs {
  max-height: 200px;
  overflow: auto;
}
</style>

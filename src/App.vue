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
                  <b-alert show>Швидке прибирання столиків у їдальні (25 столів).</b-alert>

                  <div class="buttons-list mb-2">
                    <b-button @click="randomTablePollution" class="m-1">Забруднити столи</b-button>
                    <b-button class="m-1" variant="outline-primary">Запустити агента</b-button>
                  </div>

                  <div class="table-list">
                    <div v-for="(item, id) of tableList" :key="id"
                         class="table-list-item"
                         :class="{
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
                  <div class="table-logs d-flex flex-wrap">
                    <div v-for="(message, id) of logTree" :key="id">
                      {{ message}} ->
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

    clearAllData() {
      this.initTables();
      this.logTree = [];
    },

    pushToLogTree(message) {
      this.logTree.push(message)
    }
  }
}
</script>

<style>
.tabs-list {
  min-height: 500px;
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

.table-list-item-orange{
  background: coral;
}


.table-list-item-red {
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

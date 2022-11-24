<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-12 pt-5 pb-5">
          <b-card no-body >
            <b-tabs pills card vertical class="tabs-list">
              <!-- Lab 1 -->
              <b-tab title="LAB #1" active>
                <b-card-text>Lab 1 content</b-card-text>
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
  components: {

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
</style>

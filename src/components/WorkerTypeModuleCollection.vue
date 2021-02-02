<template>
  <div class="worker-type-module">

    <h2>{{ title }}</h2>
    <p><i>{{ description }}</i></p>

    <ul class="worker_type_stats">
      <li class="worker_type" v-for="worker_type in worker_types" :key="worker_type.id">
        <ul class="stats">
          <li class="stat">
            <label :for="worker_type.name + '_salary_input'">Average Salary: </label>
            <input :id="worker_type.name + '_salary_input'" type="number" v-model="worker_type.salary">
          </li>
          <li class="stat">
            <label :for="worker_type.name + '_benefits_input'">Average Benefits: </label>
            <input :id="worker_type.name + '_cenefits_input'" type="number" v-model="worker_type.benefits">
          </li>
          <li class="stat">
            <label :for="worker_type.count + '_count_input'">Number of Staff: </label>
            <input :id="worker_type.count + '_counts_input'" type="number" v-model="worker_type.count">
          </li>
        </ul>
      </li>
    </ul>

    <div class="solution">
        <p>Adjust percentage of salary cut</p>
        <VueSlideBar :min="0" :max="100" v-model="strategies.salary_cut"></VueSlideBar>
    </div>

    <div class="solution">
        <p>Adjust percentage of benefits cut</p>
        <VueSlideBar :min="0" :max="100" v-model="strategies.benefit_cut"></VueSlideBar>
    </div>

  </div>
</template>

<script>

import VueSlideBar from 'vue-slide-bar'; 

export default {
  name: 'WorkerTypeModuleCollection',
  components: {
      VueSlideBar
  },
  props: {
    title: String,
    description: String,
    worker_types: Array
  },
  data() {
    return {
      strategies: {
        salary_cut: 0,
        benefit_cut: 0
      }
    }
  },
  methods: {
    //   
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .solution {
        margin: 15px auto 15px;
        max-width: 300px;
    }
    .solution p {
        margin: 0px;
    }
    .worker-type-module {
      margin-bottom: 100px;
    }
    .worker_type_stats {
      margin: 0px auto;
      max-width: 400px;
      text-align: center;
    }
    .worker_type_stats .worker_type {
      display: block;
    }
    .worker_type_stats .stats {
      margin-bottom: 25px;
      text-align: left;
    }
    .worker_type_stats .stats .stat {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
    }
    li {
      display: inline-block;
      margin: 0 10px;
    }
</style>

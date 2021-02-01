<template>
  <div class="worker-type-module">

    <h2>{{ worker_type.name }}</h2>
    <p><i>{{ worker_type.description }}</i></p>

    <ul class="props">
        <li class="prop">
            <label :for="worker_type.name + '_salary_input'">Average Salary: </label>
            <input :id="worker_type.name + '_salary_input'" type="number" v-model="worker_type.salary">
        </li>

        <li class="prop">
            <label :for="worker_type.name + '_benefits_input'">Average Benefits: </label>
            <input :id="worker_type.name + '_cenefits_input'" type="number" v-model="worker_type.benefits">
        </li>

        <li class="prop">
            <label :for="worker_type.count + '_count_input'">Number of Staff: </label>
            <input :id="worker_type.count + '_counts_input'" type="number" v-model="worker_type.count">
        </li>
    </ul>

    <div class="solution">
        <p>Adjust percentage of exec salary cut</p>
        <VueSlideBar :min="0" :max="100" v-model="worker_type.strategies.salary_cut" @input="emitData"></VueSlideBar>
    </div>

    <div class="solution">
        <p>Adjust percentage of exec benefits cut</p>
        <VueSlideBar :min="0" :max="100" v-model="worker_type.strategies.benefit_cut" @input="emitData"></VueSlideBar>
    </div>

  </div>
</template>

<script>

import VueSlideBar from 'vue-slide-bar'; 

export default {
  name: 'WorkerTypeModule',
  components: {
      VueSlideBar
  },
  props: {
    worker_type: Object
  },
  methods: {
    //   
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .solution {
        background-color: #eee;
        margin-bottom: 15px;
        padding: 25px;
    }

    .solution p {
        margin: 0px;
    }

    li {
    display: inline-block;
    margin: 0 10px;
    }
</style>

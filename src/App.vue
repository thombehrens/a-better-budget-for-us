<template>
  <div id="app">
    <h1>Deficit Total:</h1>
    <p>${{ deficit_total.toLocaleString() }}</p>

    <hr />

    <h2>Exec Staff Total Cost:</h2>
    <p>${{ exec_salaries_total_cost.toLocaleString() }}</p>

    <p>Adjust percentage of exec salary cut</p>
    <VueSlideBar :min="0" :max="100" v-model="strategies.paycuts.exec.salary"></VueSlideBar>

    <p>Adjust percentage of exec benefits cut</p>
    <VueSlideBar :min="0" :max="100" v-model="strategies.paycuts.exec.benefits"></VueSlideBar>


    <h2>Union Staff Total Cost:</h2>
    <p>${{ union_salaries_total_cost.toLocaleString() }}</p>

    <p>Adjust percentage of union salary cut</p>
    <VueSlideBar :min="0" :max="100" v-model="strategies.paycuts.union.salary"></VueSlideBar>

    <p>Adjust percentage of union benefits cut</p>
    <VueSlideBar :min="0" :max="100" v-model="strategies.paycuts.union.benefits"></VueSlideBar>


    <h2>Other Staff Total Cost:</h2>
    <p>${{ other_salaries_total_cost.toLocaleString() }}</p>

    <p>Adjust percentage of other salary cut</p>
    <VueSlideBar :min="0" :max="100" v-model="strategies.paycuts.other.salary"></VueSlideBar>

    <p>Adjust percentage of other benefits cut</p>
    <VueSlideBar :min="0" :max="100" v-model="strategies.paycuts.other.benefits"></VueSlideBar>
  </div>
</template>

<script>

import VueSlideBar from 'vue-slide-bar';

export default {
  name: 'App',
  components: {
    VueSlideBar
  },
  data() {
    return {
      constants: {
        annual_revenue: 153548548
      },
      strategies: {
        paycuts: {
          exec: {
            salary: 0,
            benefits: 0
          },
          union: {
            salary: 0,
            benefits: 0
          },
          other: {
            salary: 0,
            benefits: 0
          }
        }
      },
      workers: {
        types: [
          {
            name: 'exec',
            type: 'exec',
            salary: 225000,
            count: 30,
            benefits: 56250
          },
          {
            name: 'sea',
            type: 'union',
            salary: 77500,
            count: 120,
            benefits: 27125
          },
          {
            name: 'pwu_nat',
            type: 'union',
            salary: 66666,
            count: 200,
            benefits: 23333
          },
          {
            name: 'pwu_cha',
            type: 'union',
            salary: 66666,
            count: 150,
            benefits: 23333
          },
          {
            name: 'other',
            type: 'other',
            salary: 85000,
            count: 300,
            benefits: 25500
          }
        ]
      }
    }
  },
  computed: {
    deficit_total()
    {
      return this.constants.annual_revenue - (this.exec_salaries_total_cost + this.union_salaries_total_cost + this.other_salaries_total_cost)
    },
    exec_salaries_total_cost()
    {
      return this.getWorkerTypeTotal('exec');
    },
    union_salaries_total_cost()
    {
      return this.getWorkerTypeTotal('union');
    },
    other_salaries_total_cost()
    {
      return this.getWorkerTypeTotal('other');
    }
  },
  methods: {
    getWorkerTypeTotal(type, prop)
    {
      let total = 0;
      this.workers.types.filter(worker_type => worker_type.type === type).forEach(worker_type => {
        if (typeof prop === 'undefined' || prop === 'salary') {
          const salary = (this.strategies.paycuts[type].salary <= 0) ? worker_type.salary : (worker_type.salary - ((this.strategies.paycuts[type].salary/100) * worker_type.salary));
          total += salary * worker_type.count;
        }
        if (typeof prop === 'undefined' || prop === 'benefits') {
          const benefits = (this.strategies.paycuts[type].benefits <= 0) ? worker_type.benefits : (worker_type.benefits - ((this.strategies.paycuts[type].benefits/100) * worker_type.benefits));
          total += benefits * worker_type.count;
        }
      });
      return total;
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding: 100px 35%;
}
</style>

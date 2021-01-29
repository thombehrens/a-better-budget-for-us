<template>
  <div id="app">
    <h1>Total Deficit:</h1>
    <p>${{ total_deficit }}</p>
    <h1>Layoffs:</h1>
    <p>${{ layoffs }}</p>
    <h1>Average Union Salary:</h1>
    <p>${{ average_union_salary }}</p>
    <h1>Top Salary:</h1>
    <p>${{ top_salary }}</p>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      paycut: 1,
      hours: 1,
      furloughs: 0,
      furlough_vaca: 1,
      idea_name: '',
      idea_amount: '',
      projected: {
        deficit: -10000000,
        revenue: 153100000,
        salaries: 64883100,
        benefits: 20759050,
        staff: 800,
        layoffs: 42,
        avg_union_salary: 69432,
        top_salary: 300201,
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
    total_deficit()
    {
      let total = this.projected.deficit + this.projected.salaries + this.projected.benefits;
      this.workers.types.forEach((worker_type) => {
        total -= ((worker_type.salary * worker_type.count * worker_type.count) - (worker_type.benefits * worker_type.count));
      });
      return total.toLocaleString();
    },
    layoffs()
    {
      let worker_total = 0;
      this.workers.types.forEach((worker_type) => {
        worker_total -= (worker_type.salary * worker_type.count * this.hours) - (worker_type.benefits * worker_type.count);
      });
      let total = 450000 - this.projected.salaries + this.projected.benefits - worker_total / 107052.6875;

      return total.toLocaleString();
    },
    average_union_salary()
    {
      const union_types = this.workers.types.filter((worker_type) => worker_type['type'] === 'union');
      const union_totals = union_types.reduce((a,b) => (b['salary'] * this.hours * this.paycut) + a, 0);
      const union_count_totals = union_types.reduce((a,b) => b['count'] + a, 0);
      return union_totals / union_count_totals;
    },
    top_salary()
    {
      const exec = this.workers.types.find((worker_type) => worker_type['name'] === 'exec');
      return this.projected.top_salary * (exec['salary'] / 225000) * this.hours;
    }
  },
  methods: {
    
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
}
</style>

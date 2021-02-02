<template>
  <div id="app">
    <h1>Projected Deficit:</h1>
    <h3><b>${{ deficit_total.toLocaleString() }}</b></h3>
    
    <h1>Projected Staff Exits:</h1>
    <h3><b>{{ staff_exits.toLocaleString() }}</b></h3>

    <hr />

    <WorkerTypeModuleCollection
    title="Executives"
    description="Add description"
    :worker_types="getWorkerType('exec')">
    </WorkerTypeModuleCollection>

    <WorkerTypeModuleCollection
    title="Union Staff"
    description="Add description"
    :worker_types="getWorkerType('union')">
    </WorkerTypeModuleCollection>

    <WorkerTypeModuleCollection
    title="Other Staff"
    description="Add description"
    :worker_types="getWorkerType('other')">
    </WorkerTypeModuleCollection>

  </div>
</template>

<script>

import WorkerTypeModuleCollection from './components/WorkerTypeModuleCollection.vue';

export default {
  name: 'App',
  components: {
    WorkerTypeModuleCollection
  },
  data() {
    return {
      constants: {
        annual_revenue: 153100000,
        annual_expenses: 163800000,
        labor_expenses: 85642150,
        avg_compensation: 107052.69,
        avg_severance: 18096.90,
        percent_deficit_staff: 0.5,
        top_salary: 300201,
        work_days: 207
      },
      policies: {
        vacation_for_furlough: 1.0
      },
      workers: {
        types: [
          {
            id: "exec",
            name: 'Executives',
            description: 'Add description...',
            type: 'exec',
            salary: 225000,
            count: 30,
            benefits: 56250,
            strategies: {
              salary_cut: 0,
              benefit_cut: 0
            }
          },
          {
            id: "sea",
            name: 'SEA Union Workers',
            description: 'Add description...',
            type: 'union',
            salary: 77500,
            count: 120,
            benefits: 27125,
            strategies: {
              salary_cut: 0,
              benefit_cut: 0
            }
          },
          {
            id: "pwu_nat",
            name: 'National PWU Workers',
            description: 'Add description...',
            type: 'union',
            salary: 66666,
            count: 200,
            benefits: 23333,
            strategies: {
              salary_cut: 0,
              benefit_cut: 0
            }
          },
          {
            id: "pwu_chapter",
            name: 'Chapter PWU Workers',
            description: 'Add description...',
            type: 'union',
            salary: 66666,
            count: 150,
            benefits: 23333,
            strategies: {
              salary_cut: 0,
              benefit_cut: 0
            }
          },
          {
            id: "other",
            name: 'Other Staff',
            description: 'Add description...',
            type: 'other',
            salary: 85000,
            count: 300,
            benefits: 25500,
            strategies: {
              salary_cut: 0,
              benefit_cut: 0
            }
          }
        ]
      }
    }
  },
  computed: {
    deficit_total()
    {
      return (this.constants.annual_revenue - this.constants.annual_expenses) + (this.constants.labor_expenses - (this.exec_salaries_total_cost + this.union_salaries_total_cost + this.other_salaries_total_cost))
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
    },
    staff_exits()
    {
      return (this.deficit_total / (this.constants.avg_compensation - this.constants.avg_severance)) * this.constants.percent_deficit_staff
    }
  },
  methods: {
    getWorkerType(type)
    {
      return this.workers.types.filter(worker_type => worker_type.type === type);
    },
    getWorkerTypeTotal(type, prop)
    {
      let total = 0;
      this.workers.types.filter(worker_type => worker_type.type === type).forEach(worker_type => {
        if (typeof prop === 'undefined' || prop === 'salary') {
          const salary = (worker_type.strategies.salary_cut <= 0) ? worker_type.salary : (((100-worker_type.strategies.salary_cut)/100) * worker_type.salary);
          total += salary * worker_type.count;
        }
        if (typeof prop === 'undefined' || prop === 'benefits') {
          const benefits = (worker_type.strategies.benefit_cut <= 0) ? worker_type.benefits : (((100-worker_type.strategies.benefit_cut)/100) * worker_type.benefits);
          total += benefits * worker_type.count;
        }
      });
      return total;
    }
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
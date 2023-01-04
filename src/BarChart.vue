<template>
    <Bar id="my-chart-id" :options="chartOptions" :data="chartData" />
</template>

<script>
import {Bar} from 'vue-chartjs'
import {Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, Colors} from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, Colors)

export default {
  name: 'BarChart',
  components: { Bar },
  data() {
    return {
      chartData: {
        labels: [ 'January', 'February', 'March' ],
        datasets: [ { data: [40, 12, 12] , label: 'Group A'},
        { data: [40, 45, 12] , label: 'Group B'},
        { data: [40, 20, 28] , label: 'Group C'} ],
        
      },
      chartOptions: {
        responsive: true,
        plugins: {title: {text: 'Spending by Month', display: true}},
        scales: {
            x: {
                stacked: true
            },
            y: {
                stacked: true
            }
        }
      }
    }
  }
}
</script>
<template>
  <div>
    <!-- dataPointSelection emit is used to detect clicks on a pie slice to adjust the drill state -->
    <apexchart
      width="500"
      type="pie"
      :options="{ labels: chartLabels }"
      :series="chartSeries"
      @dataPointSelection="chartDrill"
    ></apexchart>
    
    <button @click="drill = -1" v-if="drill !== -1">Back</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //variable to track drill state.  -1 for no drill, otherwise it's set to the index of the drilled object
      drill: -1,

      //summary and detail data sets, separated for clarity.  Detail arrays are arranged to match their summary data element indexes
      summaryLabels: ["Group A", "Group B", "Group C"],
      summarySeries: [100, 150, 200],
      detailLabels: [
        ["A1", "A2", "A3", "A4", "A5"],
        ["B1", "B2", "B3", "B4", "B5", "B6"],
        ["C1", "C2"],
      ],
      detailSeries: [
        [10, 10, 20, 20, 40],
        [5, 10, 10, 25, 25, 75],
        [150, 50],
      ],
    };
  },
  methods: {
    /*
      chartDrill method acts as dataPointSelection event handler, setting app state variable to track the drill target.  
      We're only supporting one level of drilling, so if we're not at the top level the handler ignores the event.
    */
    chartDrill(event, context, config) {
      if (this.drill === -1) {
        this.drill = config.dataPointIndex;
      }
    },
  },
  computed: {
    /*
      chartLabels and chartSeries select the array to be passed to the chart, depending on the drill state variable
    */
    chartLabels() {
      if (this.drill === -1) {
        return this.summaryLabels;
      } else {
        return this.detailLabels[this.drill];
      }
    },
    chartSeries() {
      if (this.drill === -1) {
        return this.summarySeries;
      } else {
        return this.detailSeries[this.drill];
      }
    },
  },
};
</script>

<template>
  <div>
    <!-- dataPointSelection emit is used to detect clicks on a pie slice to adjust the drill state -->
    <apexchart
      width="400"
      type="pie"
      :options="chartOptions"
      :series="chartSeries"
      @dataPointSelection="chartDrill"
    ></apexchart>

    <button
      @click="chartDrill(null, null, { dataPointIndex: -1 })"
      v-if="drill !== -1"
    >
      Back
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //variable to track drill state.  -1 for no drill, otherwise it's set to the index of the drilled object
      drill: -1,
      chartSeries: [],
      chartOptions: {},

      //summary and detail data sets, separated for clarity.  Detail arrays are arranged to match their summary data element indexes
      summaryLabels: ["Group A", "Group B", "Group C"],
      summarySeries: [100, 150, 200],
      detailLabelsA: ["A1", "A2", "A3", "A4", "A5"],
      detailLabels: [
        ["A1", "A2", "A3", "A4", "A5"],
        ["B1", "B2", "B3", "B4", "B5", "B6"],
        ["C1", "C2"],
      ],
      detailSeriesA: [10, 10, 20, 20, 40],
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
      console.log(config);
      if (this.drill === -1 || config.dataPointIndex === -1) {
        this.drill = config.dataPointIndex;

        this.chartSeries = this.setChartSeries();
        this.chartOptions = {...this.chartOptions, ...{labels: this.setChartLabels() }};
      }
    },
    /*
      chartLabels and chartSeries select the array to be passed to the chart, depending on the drill state variable
    */
    setChartLabels() {
      if (this.drill === -1) {
        return this.summaryLabels;
      } else {
        return this.detailLabels[this.drill];
      }
    },
    setChartSeries() {
      if (this.drill === -1) {
        return this.summarySeries;
      } else {
        return this.detailSeries[this.drill];
      }
    },
  },
  beforeMount() {
    this.chartSeries = this.setChartSeries();
    this.chartOptions = { labels: this.setChartLabels() };
  },
};
</script>

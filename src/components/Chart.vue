<script lang="ts">
import Vue, { PropType } from 'vue';
import { ChartData, ChartOptions } from 'chart.js';
import { Line, mixins } from 'vue-chartjs';

export default Vue.extend({
  extends: Line,
  props: {
    chartData: {
      type: Object as PropType<ChartData>,
    },
    options: {
      type: Object as PropType<ChartOptions>,
    },
  },

  mixins: [Line, mixins.reactiveProp],

  mounted: function() {
    (this as any).renderChart(this.chartData, this.options);
  },
});
</script>

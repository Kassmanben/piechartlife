<script>
import { Pie,  mixins} from 'vue-chartjs'
export default {
  name: "Chart",
  extends: Pie,
  mixins: [mixins.reactiveProp],
  props: ["chartData"],
  mounted () {
    // Overwriting base render method with actual data.
    this.renderChart(this.chartData)
  }
}
</script>


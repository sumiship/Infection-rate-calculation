<template>
  <div class="Statistics">
    <p>view</p>
    <p>{{ values }}</p>
    <table>
      <tr>
        <th>陽性率</th>
        <td>{{ ratePositive }} %</td>
      </tr>
      <tr>
        <th>罹患in陽性</th>
        <td>{{ rateMorbidityInPositive }} %</td>
      </tr>
    </table>

    <Chart
      class="chart"
      :sample="values.sample"
      :morbidity="values.morbidity"
      :sensitivity="values.sensitivity"
      :specificity="values.specificity"
      :select="values.select"
    />
    <SubChart
      class="chart"
      :sample="values.sample"
      :morbidity="values.morbidity"
      :sensitivity="values.sensitivity"
      :specificity="values.specificity"
      :select="values.select"
    />
  </div>
</template>
<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";
import Chart from "@/components/Char.vue";
import SubChart from "@/components/SubChar.vue";

interface IPropValues {
  positive: number;
  morbidity: number;
  sample: number;
  sensitivity: number;
  specificity: number;
  select: number;
}

@Component({
  components: {
    Chart,
    SubChart,
  },
})
export default class Input extends Vue {
  @Prop()
  private values!: IPropValues;

  get ratePositive(): number {
    const num: number =
      (this.values.positive * 10000) / this.values.sample / 100;
    return Number(num.toFixed(2));
  }
  get rateMorbidityInPositive(): number {
    const num: number =
      (((this.values.morbidity / 100) * this.values.sensitivity) /
        100 /
        (((this.values.morbidity / 100) * this.values.sensitivity) / 100 +
          (1 - this.values.morbidity / 100) *
            (1 - this.values.specificity / 100))) *
      100;
    return Number(num.toFixed(2));
  }
}
</script>
<style scoped>
.chart {
  max-width: 700px;
  width: 90%;
  margin: 0 auto;
}
</style>

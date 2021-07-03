<template>
  <div class="Statistics">
    <p>view</p>
    <p>罹患in陽性: {{ rateMorbidityInPositive }} %</p>
    <p>予想陽性率: {{ ratePositive }} %</p>
    <button @click="witchChart = !witchChart">グラフチェンジ</button>

    <Chart
      v-show="witchChart == 1"
      class="chart"
      :sample="values.sample"
      :morbidity="values.morbidity"
      :sensitivity="values.sensitivity"
      :specificity="values.specificity"
      :select="values.select"
    />
    <div v-show="!witchChart">
      <button @click="callChild()">view</button>
      <SubChart
        ref="child"
        class="chart"
        :sample="values.sample"
        :morbidity="values.morbidity"
        :sensitivity="values.sensitivity"
        :specificity="values.specificity"
        :select="values.select"
      />
    </div>
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
  private witchChart = true;

  get ratePositive(): number {
    const num: number =
      (((this.values.morbidity / 100) * this.values.sensitivity) / 100 +
        (1 - this.values.morbidity / 100) *
          (1 - this.values.specificity / 100)) *
      100;
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
  private callChild(): void {
    const refsChild: any = this.$refs.child;
    refsChild.update();
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

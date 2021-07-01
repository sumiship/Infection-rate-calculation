<template>
  <div class="home">
    <Input :values="values" />
    <Statistics :values="values" />
  </div>
</template>

<script lang="ts">
import { Component, Vue, Watch } from "vue-property-decorator";
import Input from "@/components/Input.vue";
import Statistics from "@/components/Statistics.vue";

@Component({
  components: {
    Input,
    Statistics,
  },
})
export default class Home extends Vue {
  private values = {
    positive: 1000,
    sample: 2000,
    sensitivity: 70,
    specificity: 99,
    morbidity: 1,
    select: 3,
  };
  // changeSelect() {
  //   console.log("a");
  // }

  @Watch("values.select")
  onSelectCanged() {
    console.log(this.values.select);
  }
}
</script>

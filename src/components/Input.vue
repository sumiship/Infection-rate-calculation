<template>
  <div class="input">
    <div class="in">
      <p>陽性者数(人)</p>
      <input type="number" v-model="values.positive" />
    </div>
    <div class="in">
      <p>検査数(人)</p>
      <input type="number" v-model="values.sample" />
    </div>
    <div class="in">
      <p @click="selectVariable(1)" :class="selectClass(1)">感度(%)</p>
      <input type="number" v-model="values.sensitivity" min="0" max="100" />
    </div>
    <div class="in">
      <p @click="selectVariable(2)" :class="selectClass(2)">特異度(%)</p>
      <input type="number" v-model="values.specificity" min="0" max="100" />
    </div>
    <div class="in">
      <p @click="selectVariable(3)" :class="selectClass(3)">罹患率(%)</p>
      <input type="number" v-model="values.morbidity" min="0" max="100" />
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";

interface IPropValues {
  positive: number;
  morbidity: number;
  sample: number;
  sensitivity: number;
  specificity: number;
  select: number;
}

@Component({})
export default class Input extends Vue {
  @Prop()
  private values!: IPropValues;

  private select: number = this.values.select;
  // @Emit("change")
  // public changeSelect(select: number) {
  //   console.log(select);
  // }
  private selectVariable(id: number): void {
    this.values.select = id;
    this.select = id;
    // this.changeSelect(id);
  }

  private selectClass(id: number): string {
    if (this.values.select == id) return "select";
    // console.log(this);
  }
}
</script>
<style scoped>
.input {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}
.in {
  width: 180px;
}
.select {
  background-color: coral;
}
input {
  width: 100px;
}
</style>

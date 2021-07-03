<script>
import { Bar } from "vue-chartjs";

export default {
  extends: Bar,
  name: "chart",
  props: ["sample", "morbidity", "sensitivity", "specificity", "select"],
  data() {
    return {
      data: {
        labels: ["January", "February", "March", "April", "May", "June"],
        datasets: [
          {
            label: "yousei",
            data: [10, 50, 20, 30, 30, 40],
            borderColor: "#CFD8DC",
            fill: false,
            type: "line",
            lineTension: 0.3,
          },
        ],
      },
      options: {
        animation: false,
        scales: {
          xAxes: [
            {
              scaleLabel: {
                display: true,
                labelString: "",
              },
            },
          ],
          yAxes: [
            {
              ticks: {
                beginAtZero: true,
                stepSize: 10,
              },
            },
          ],
        },
      },
    };
  },
  // watch: {
  //   sensitivity: function() {
  //     this.update();
  //   },
  //   morbidity: function() {
  //     this.update();
  //   },
  //   specificity: function() {
  //     this.update();
  //   },
  //   select: function() {
  //     this.update();
  //   },
  // },
  methods: {
    update() {
      this.yousei();
      this.renderChart(this.data, this.options);
      // console.log(this.make1000arr());
      // console.log(this.data.datasets[0].data);
      // console.log(this.data.labels);
    },
    yousei() {
      let youseiView = [];
      for (let j = 0; j < 10000; j++) {
        let count = 0;
        for (let i = 0; i < this.sample; i++) {
          let judge = false;
          if (Math.random() * 100 < this.morbidity) judge = true;
          if (judge) {
            if (Math.random() * 100 >= this.sensitivity) judge = false;
          } else {
            if (Math.random() * 100 >= this.specificity) judge = true;
          }
          if (judge) count++;
        }
        youseiView.push(count);
      }
      const classify = this.classifyArr(youseiView);
      this.data.datasets[0].data = JSON.parse(JSON.stringify(classify[1]));
      this.data.labels = JSON.parse(JSON.stringify(classify[0]));
      this.renderChart(this.data, this.options);
    },
    classifyArr(arr) {
      let retArr = [];
      let labelArr = [];
      arr.sort();
      const min = arr[0];
      const max = arr[arr.length - 1];
      // console.log(min, max);
      for (let i = min; i <= max; i++) {
        labelArr.push(i);
        let count = arr.filter((item) => item == i).length;
        retArr.push(count);
      }
      // console.log(retArr);
      return [labelArr, retArr];
    },
  },
  mounted() {
    this.update();
    // this.renderChart(this.data, this.options);
  },
};
</script>

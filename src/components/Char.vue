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
            label: "Line Dataset",
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
                max: 100,
                min: 0,
              },
            },
          ],
        },
      },
    };
  },
  watch: {
    sensitivity: function() {
      this.update();
    },
    morbidity: function() {
      this.update();
    },
    specificity: function() {
      this.update();
    },
    select: function() {
      this.update();
    },
  },
  methods: {
    make1000arr() {
      let ret = [];
      for (let i = 0; i < 1000; i++) {
        ret.push(i / 10);
      }
      return ret;
    },
    update() {
      this.setView(this.select);
      // console.log(this);
      // const newChartData = Object.assign({}, this.chartData);
      // newChartData.datasets[0].data = this.views;
      // console.log(this.chartData);
      // this.chartData = this.views;
      this.data.datasets[0].data = JSON.parse(JSON.stringify(this.views));
      this.data.labels = JSON.parse(JSON.stringify(this.make1000arr()));
      this.renderChart(this.data, this.options);
      // console.log(this.make1000arr());
      // console.log(this.data.datasets[0].data);
      // console.log(this.data.labels);
    },
    rateMorbidityInPositive(mo, nomo) {
      return (mo / (mo + nomo)) * 100;
    },
    setView(select) {
      this.views = [];
      if (select == 1) {
        for (let i = 0; i < 1000; i++) {
          let mo = ((this.morbidity / 100) * i) / 1000;
          let nomo = (1 - this.morbidity / 100) * (1 - this.specificity / 100);
          this.views.push(this.rateMorbidityInPositive(mo, nomo));
        }
      } else if (select == 2) {
        for (let i = 0; i < 1000; i++) {
          let mo = ((this.morbidity / 100) * this.sensitivity) / 100;
          let nomo = (1 - this.morbidity / 100) * (1 - i / 1000);
          this.views.push(this.rateMorbidityInPositive(mo, nomo));
        }
      } else if (select == 3) {
        for (let i = 0; i < 1000; i++) {
          let mo = ((i / 1000) * this.sensitivity) / 100;
          let nomo = (1 - i / 1000) * (1 - this.specificity / 100);
          this.views.push(this.rateMorbidityInPositive(mo, nomo));
        }
      }
    },
  },
  mounted() {
    this.update();
    // this.renderChart(this.data, this.options);
  },
};
</script>

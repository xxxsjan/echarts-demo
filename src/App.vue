<template>
  <div id="echarts_main" style="
      width: 80%;
      height: 600px;
      backgroundColor: #000;
      margin: 0 auto;
      opcity: 0.5;
    "></div>
</template>

<script>
export default {
  name: "rwwgEcharts",
  data() {
    return {
      chart: null,
      data: [],
      timer: null,
      seriesData: []
    };
  },
  /* 自定义缩放指令 */
  directives: {
    resize: {
      bind(el, binding) {
        let width = "",
          height = "";

        function isReize() {
          const style = document.defaultView.getComputedStyle(el);
          if (width !== style.width || height !== style.height) {
            binding.value();
          }
          width = style.width;
          height = style.height;
        }
        el.__vueSetInterval__ = setInterval(isReize, 5);
      },
      unbind(el) {
        clearInterval(el.__vueSetInterval__);
      },
    },
  },
  computed: {},
  mounted() {
    this.initChart();
  },
  methods: {
    async initChart() {
      this.chart = this.$echarts.init(document.getElementById("echarts_main"));

      this.data = await this.getData()
      this.seriesData = this.getSeriesData(0);
      const xAxisData = this.data.list.map(m => m[0].x)
      let option = {
        title: {
          text: "任务网格图",
          textStyle: {
            color: "white",
          },
        },
        legend: {
          data: ["原任务1", "原任务2", "原任务3", "原任务4", "原任务5"],
          textStyle: {
            color: "white",
          },
        },
        tooltip: {
          // formatter: function (params) {
          //     return 'X: ' + params.data[0].toFixed(2) + '<br>Y: ' + params.data[1].toFixed(2);
          // },
          trigger: "axis",
          axisPointer: {
            type: "cross",
          },
        },
        grid: {},
        xAxis: {
          // min: 0,
          // max: 50,
          // type: "value",
          axisLine: { onZero: false },
          color: "#00ffff",
          type: "category",
          data: xAxisData,
        },
        axisLabel: {
          // formatter: '{value}位',
          show: true,
          color: "#00ffff",
          // formatter: (val) => {
          //   console.log(val);
          //   return "T" + `${-val}`;
          // },
        },

        yAxis: {
          type: "category",
          data: ["执行失败", "执行成功", "下达成功", "待下发", "待执行"],
          axisLabel: {
            color: "#00ffff",
            formatter: (val) => {
              return `${val}`;
            },
          },
        },
        dataZoom: [
          {
            type: "slider",
            xAxisIndex: 0,
            filterMode: "empty",
            textStyle: {
              color: "white",
            },
          },
          {
            type: "slider",
            yAxisIndex: 0,
            filterMode: "empty",
            textStyle: {
              color: "white",
            },
          },
          {
            type: "inside",
            xAxisIndex: 0,
            filterMode: "empty",
          },
          {
            type: "inside",
            yAxisIndex: 0,
            filterMode: "empty",
          },
        ],
        series: this.seriesData
      };

      this.chart.setOption(option);

      this.runTimer()

      // 根据页面大小自动响应图表大小
      window.addEventListener("resize", () => {
        this.chart.resize();
      });
    },

    runTimer() {
      const len = this.data.length
      let i = 0
      this.timer = setInterval(() => {
        i++
        const seriesData = this.getSeriesData(i)
        this.chart.setOption({
          series: seriesData
        });
        if (i === len) {
          clearInterval(this.timer)
        }
      }, 2000);
    },
    getSeriesData(idx) {
      let symbolSize = 20;

      function createSeries(list) {
        const colors = ['#00ffff', '#FFFE27', '#F58787', '#CDF296', '#D1AEEC', '#B2D6FF', '#FFB2B2', '#FFD1AEEC', '#FFB2D6FF'];
        return list.map((item, index) => {
          return {
            name: "原任务" + (index + 1),
            id: index,
            type: "line",
            smooth: true,
            symbolSize: symbolSize,
            data: item.map(function (it) {
              return [index <= idx ? it.x : '', it.y];
            }),
            itemStyle: {
              borderRadius: [0, 10, 10, -3],
              color: colors[index],
              // borderWidth: 1,
              // borderColor: "skyblue"
            },
            label: {
              show: true,
              formatter: function (params) {
                return params.value[1]
              },
            },
          }
        })
      }
      const result = createSeries(this.data.list, idx)
      return result
    },
    getData() {
      return new Promise((resolve) => {
        let data = [
          { x: '原任务1', y: "待执行" },
          { x: '原任务1', y: "执行成功" },
        ];
        let data2 = [
          { y: "待执行", x: '原任务2' },
          { y: "待下发", x: '原任务2' },
          { y: "下达成功", x: '原任务2' },
          { y: "执行成功", x: '原任务2' },
        ];
        let data3 = [
          { y: "待下发", x: '原任务3' },
          { y: "下达成功", x: '原任务3' },
          { y: "执行失败", x: '原任务3' },
        ];
        let data4 = [
          { y: "待执行", x: '原任务4' },
          { y: "执行成功", x: '原任务4' },
        ];
        let data5 = [
          { y: "待下发", x: '原任务5' },
          { y: "下达成功", x: '原任务5' },
          { y: "执行成功", x: '原任务5' },
        ];
        const res = {
          list: [
            data,
            data2,
            data3,
            data4,
            data5
          ]
        }
        setTimeout(() => {
          resolve(res)
        }, 100);
      })
    },
    resize() {
      let charts = this.$echarts.init(document.getElementById("echarts_main"));
      this.$nextTick(() => {
        charts.resize();
      });
    },
  },
};
</script>

<style scoped></style>

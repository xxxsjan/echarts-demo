<template>
  <div
    id="echarts_main"
    style="
      width: 80%;
      height: 600px;
      backgroundColor: #000;
      margin: 0 auto;
      opcity: 0.5;
    "
    v-resize="resize"
  ></div>
</template>

<script>
export default {
  name: "rwwgEcharts",
  data() {
    return {};
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
    this.getRwwg();
  },
  methods: {
    getRwwg() {
      let lb1 = ["待执行", "执行成功"];
      let lb2 = ["待执行", "待下发", "下达成功", "执行成功"];
      let lb3 = ["待下发", "下达成功", "执行失败"];
      let lb4 = ["待执行", "执行成功"];
      let lb5 = ["待下发", "下达成功", "执行成功"];

      let rwwgCharts = this.$echarts.init(
        document.getElementById("echarts_main")
      );

      let symbolSize = 20;
      let data = [
        { x: 3, y: "待执行" },
        { x: 22, y: "执行成功" },
      ];
      let data2 = [
        { y: "待执行", x: 3 },
        { y: "待下发", x: 13 },
        { y: "下达成功", x: 26 },
        { y: "执行成功", x: 33 },
      ];
      let data3 = [
        { y: "待下发", x: 5 },
        { y: "下达成功", x: 20 },
        { y: "执行失败", x: 30 },
      ];
      let data4 = [
        { y: "待执行", x: 3 },
        { y: "执行成功", x: 40 },
      ];
      let data5 = [
        { y: "待下发", x: 9 },
        { y: "下达成功", x: 28 },
        { y: "执行成功", x: 42 },
      ];

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
          min: 0,
          max: 50,
          type: "value",
          axisLine: { onZero: false },
          color: "#00ffff",
        },
        axisLabel: {
          // formatter: '{value}位',
          show: true,
          color: "#00ffff",
          formatter: (val) => {
            console.log(val);
            return "T" + `${-val}`;
          },
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
        series: [
          {
            name: "原任务1",
            id: "a",
            type: "line",
            smooth: true,
            symbolSize: symbolSize,
            data: data.map(function (item) {
              return [item.x, item.y];
            }),
            itemStyle: {
              borderRadius: [0, 10, 10, -3],
              color: "#00ffff",
              // borderWidth: 1,
              // borderColor: "skyblue"
            },
            label: {
              show: true,
              formatter: function (params) {
                return lb1[params.dataIndex];
              },
            },
          },
          {
            name: "原任务2",
            id: "b",
            type: "line",
            smooth: true,
            symbolSize: symbolSize,
            data: data2.map(function (item) {
              return [item.x, item.y];
            }),
            itemStyle: {
              borderRadius: [0, 10, 10, -3],
              color: "#FFFE27",
              // borderWidth: 1,
              // borderColor: "skyblue"
            },
            label: {
              show: true,
              formatter: function (params) {
                return lb2[params.dataIndex];
              },
            },
          },
          {
            name: "原任务3",
            id: "c",
            type: "line",
            smooth: true,
            symbolSize: symbolSize,
            data: data3.map(function (item) {
              return [item.x, item.y];
            }),
            itemStyle: {
              borderRadius: [0, 10, 10, -3],
              color: "#F58787",
            },
            label: {
              show: true,
              formatter: function (params) {
                return lb3[params.dataIndex];
              },
            },
          },
          {
            name: "原任务4",
            id: "d",
            type: "line",
            smooth: true,
            symbolSize: symbolSize,
            data: data4.map(function (item) {
              return [item.x, item.y];
            }),
            itemStyle: {
              borderRadius: [0, 10, 10, -3],
              color: "#CDF296",
            },
            label: {
              show: true,
              formatter: function (params) {
                return lb4[params.dataIndex];
              },
            },
          },
          {
            name: "原任务5",
            id: "e",
            type: "line",
            smooth: true,
            symbolSize: symbolSize,
            data: data5.map(function (item) {
              return [item.x, item.y];
            }),
            itemStyle: {
              borderRadius: [0, 10, 10, -3],
              color: "#D1AEEC",
            },
            label: {
              show: true,
              formatter: function (params) {
                return lb5[params.dataIndex];
              },
            },
          },
        ],
      };

      this.rwwg = option;

      rwwgCharts.setOption(option);

      // 根据页面大小自动响应图表大小
      //   window.addEventListener("resize", () => {
      //     rwwgCharts.resize();
      //   });
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

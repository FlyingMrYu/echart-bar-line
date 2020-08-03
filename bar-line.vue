<template>
  <div class="bar-chart-content">
    <div :id="'chartpieBox'+id" :style="{width: '100%',height: `${height}px`}"></div>
  </div>
</template>
<script>
import echarts from "echarts";
export default {
  name: "barsharePieChart",
  props: ["id", "height", "middleTotalData"],
  watch: {
    middleTotalData() {
      if (this.middleTotalData && this.middleTotalData.legendData) {
        this.init();
      }
    }
  },
  methods: {
    init() {
      this.chartLine = echarts.init(
        document.getElementById(`chartpieBox${this.id}`)
      );
      var option = {};
      console.log(
        this.middleTotalData.legendData,
        "this.middleTotalData.legendData"
      );
      if (this.middleTotalData.legendData.length != 0) {
        option = {
          tooltip: {
            trigger: "axis"
          },
          grid: {
            top: 50,
            bottom: 20,
            left: 60,
            right: 60
          },
          legend: {
            right: 40,
            textStyle: {
              color: "#7c9fc7"
            },
            data: this.middleTotalData.legendData
          },
          xAxis: [
            {
              type: "category",
              data: this.middleTotalData.xAxisData,
              axisPointer: {
                type: "shadow"
              },
              axisTick: { show: false },
              axisLine: {
                //坐标轴轴线相关设置。
                lineStyle: {
                  color: "#7c9fc7"
                },
                show: false
              }
            }
          ],
          color: ["#ffbc1b", "#5790ff"],
          yAxis: [
            {
              type: "value",
              name: "能耗值",
              min: 0,
              // interval: 5,
              axisLine: {
                //坐标轴轴线相关设置。
                lineStyle: {
                  color: "#7c9fc7"
                },
                show: false
              },
              axisTick: { show: false },
              splitLine: {
                lineStyle: {
                  type: "dashed",
                  color: ["#5f7b9e"] //设置网格线类型 dotted：虚线   solid:实线
                },
                show: true //隐藏或显示
              }
            },
            {
              type: "value",
              name: "",
              min: 0,
              // show: false,
              // interval: 50,
              axisLine: {
                //坐标轴轴线相关设置。
                lineStyle: {
                  color: "#7c9fc7"
                },
                show: false
              },
              axisTick: { show: false },
              splitLine: {
                show: false
              }
            }
          ],
          series: this.middleTotalData.seriesData
        };
      } else {
        this.chartLine.clear();
        return;
      }
      // 使用刚指定的配置项和数据显示图表。
      this.chartLine.setOption(option);
      var _that = this;
      window.onresize = function() {
        _that.chartLine.resize();
      };
    }
  },
  mounted() {
    if (this.middleTotalData && this.middleTotalData.legendData) {
      this.init();
    }
  }
};
</script>

<style scoped lang="scss">
.bar-chart-content {
}
</style>

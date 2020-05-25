<template>
  <div class="main">
    <el-button-group>
      <el-button type="primary" plain @click="handleRefresh">刷新散点图</el-button>
      <el-button type="primary" plain @click="play">播放</el-button>
      <el-button type="primary" plain @click="pause">暂停</el-button>
    </el-button-group>

    <div ref="content" class="content"></div>
  </div>
</template>

<script>
import echarts from 'echarts';
import 'echarts-gl';

// 刷新周期时间 单位毫秒
const period = 1000;
// 点亮后的颜色
const active_color = 'red';
// 点默认的颜色
const default_color = 'green';

export default {
  name: 'Home',

  data(){
    return {
      // echarts 默认配置
      echartsOption: {
          grid3D: {},
          xAxis3D: {},
          yAxis3D: {},
          zAxis3D: {},
          series: [
              {
                type: 'scatter3D',
                // 如果跳到该点，增加它的大小
                symbolSize: (data, params) => { return params.color === active_color ?  15 : 10},
                data: this.getDataSource()
              }
          ]
      },
      // 当前变色的点序号
      currentPointIndex: 0
    }
  },

  mounted() {
    // 初始化
    this.initEcharts();
  },

  methods: {
    /**
     * 初始化Echarts
     */
    initEcharts(){
      // 初始化echarts实例
      this.myChart = echarts.init(this.$refs.content);

      this.myChart.setOption(this.echartsOption);
    },

    /**
     * 获取数据源
     */
    getDataSource(){
      let data = [];
      let count = this.random(30);

      for (let index = 0; index < count; index++) {
        data.push(
          {
            value : [this.random(10), this.random(10), this.random(10)],
            itemStyle: {
              color: default_color
            }
          }
        );
      }
      
      return data;
    },

    /**
     * 生成随机整数
     * @param {Number} max 1-max内的整数
     * @returns 指定单位内的随机整数
     */
    random(max){
      return Math.floor(Math.random() * max + 1);
    },

    /**
     * 刷新散点图
     */
    handleRefresh(){
      if(this.myChart){
        // 刷新前先停止
        this.pause();
        // 还原当前变色的点序号
        this.currentPointIndex = 0;

        // 重新加载数据源
        let option = this.myChart.getOption();
        option.series[0].data = this.getDataSource();
        this.myChart.setOption(option); 
      }
    },

    /**
     * 播放
     */
    play(){
      this.timer = window.setInterval(() => {
        let option = this.myChart.getOption();
        let oldData = option.series[0].data;
  
        // 跳到最后一点的时候重新回到第一点
        if(this.currentPointIndex >= oldData.length){
          this.currentPointIndex = 0;
        }
  
        // 给当前点样式，其他的还原
        let newData = oldData.map((item, index) => {
          if(index == this.currentPointIndex){
            item.itemStyle.color = active_color;
          }else{
            item.itemStyle.color = default_color;
          }
  
          return item;
        });
        
        // 给图标设置新的数据
        option.series[0].data = newData;
        this.myChart.setOption(option); 
  
        // 下一点
        this.currentPointIndex++;
      }, period);
    },

    /**
     * 暂停
     */
    pause(){
      if(this.timer){
        window.clearInterval(this.timer);
      }
    }
  },
  beforeDestroy(){
    this.pause();
  }
}
</script>

<style>
  .main {
    text-align: center;
  }

  .content {
    height: 700px;
  }
</style>

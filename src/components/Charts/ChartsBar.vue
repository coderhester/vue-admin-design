<template>
  <div ref="dom" class="charts-bar"></div>
</template>

<script>
import echarts from 'echarts'

export default {
  name: 'ChartsBar',
  data () {
    return {
      dom: null
    }
  },
  props: {
    title: {
      type: String,
      default: ''
    },
    data: {
      type: Object,
      default: () => {}
    }
  },
  mounted () {
    this.drawing()
  },
  beforeDestroy () {
    window.removeEventListener('resize', this.resize)
  },
  methods: {
    resize () {
      this.dom.resize()
    },
    drawing () {
      let xAxisData = Object.keys(this.data)
      let seriesData = Object.values(this.data)
      let options = {
        // 标题
        title: {
          text: this.title,
          left: 'center',
          top: 15,
          textStyle: {
            fontSize: 16,
            fontWeight: 'normal'
          }
        },
        // 工具提示
        tooltip: {
          trigger: 'axis',
          formatter: '{b} <br/>{a} : {c}',
          axisPointer: {
            type: 'shadow'
          }
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        },
        xAxis: [
          {
            type: 'category',
            data: xAxisData,
            axisTick: {
              alignWithLabel: true
            }
          }
        ],
        yAxis: [
          {
            type: 'value',
            max: value => {
              return Math.ceil(value.max / 100) * 100 + 300
            }
          }
        ],
        // 颜色配置
        color: ['#2d8cf0'],
        // 类型配置
        series: [
          {
            name: '直接访问',
            // 类型配置
            type: 'bar',
            barWidth: '40%',
            // 数据配置
            data: seriesData,
            itemStyle: {
              normal: {
                label: {
                  show: true,
                  fontSize: 14,
                  position: 'top',
                  formatter: '{c}'
                }
              }
            }
          }
        ]
      }
      this.dom = echarts.init(this.$refs.dom)
      this.dom.setOption(options)
      window.addEventListener('resize', this.resize)
    }
  }
}
</script>

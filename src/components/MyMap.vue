//EchartsBaiduMap.vue
<template>
  <div
    id="mapEcharts"
    style="width: 80%; height: 100%; position: absolute;"
  ></div>
  <div v-show="showInfoWindow" id="infoWindow" class="echarts-info-label-item" >
    <p id="infoTitle">{{cityName}}</p>
    <p id="infoContent">{{cityInfo[cityName]}}</p>
    <span v-show="showInfoWindow" id="closeBtn" ref="closeBtn" @click="hideInfoWindow">X</span>
  </div>
</template>

<script>
import * as echarts from 'echarts'
import 'echarts/extension/bmap/bmap' // bmap用来处理百度地图

export default {
  data () {
    return {
      vals: [
        { name: '昆明', value: 11 },
        { name: '普洱', value: 42 },
        { name: '红河', value: 22 },
        { name: '文山', value: 5 },
        { name: '临沧', value: 22 },
        { name: '瑞丽', value: 14 },
        { name: '昭通', value: 5 },
        { name: '大理', value: 44 },
        { name: '保山', value: 4 },
        { name: '怒江', value: 26 }
      ],
      geoVals: {
        昆明: [102.73, 25.04],
        普洱: [100.97257, 22.83098],
        红河: [103.38155, 23.37],
        文山: [104.22257, 23.40599],
        临沧: [100.09544, 23.89047],
        瑞丽: [97.86184, 24.02397],
        昭通: [103.72351, 27.34408],
        大理: [100.30807, 25.68461],
        保山: [100.385, 26.01412],
        怒江: [98.86329, 26.89]
      },
      myMap: null,
      showInfoWindow: false,
      cityInfo: {
        昆明: '云南省省会,西南交通中心...',
        普洱: '云南省西部城市,澜沧江东岸...'
        // 其他城市介绍...
      }
    }
  },
  methods: {
    convertData (data) {
      const res = []
      for (let i = 0; i < data.length; i++) {
        const geoCoord = this.geoVals[data[i].name]
        if (geoCoord) {
          res.push({
            name: data[i].name,
            value: geoCoord.concat(data[i].value)
          })
        }
      }
      return res
    },
    initEcharts () {
      this.myMap = echarts.init(document.getElementById('mapEcharts'))
      // const myMap = echarts.init(document.getElementById('mapEcharts'))
      const option = {
        title: {
          text: '全省调查图像数据总览',
          left: 'center',
          top: '10px',
          textStyle: {
            color: '#fff'
          }
        },
        // 使用echarts配置地图
        bmap: {
          center: [101, 25],
          zoom: 8,
          roam: false,
          areaLeval: 1,
          mapStyle: {
            styleJson: [
              {
                featureType: 'water',
                elementType: 'all',
                stylers: {
                  color: '#044161'
                }
              },
              {
                featureType: 'land',
                elementType: 'all',
                stylers: {
                  color: '#004981'
                }
              },
              {
                featureType: 'boundary',
                elementType: 'geometry',
                stylers: {
                  color: '#064f85'
                }
              },
              {
                featureType: 'railway',
                elementType: 'all',
                stylers: {
                  visibility: 'off'
                }
              },
              {
                featureType: 'highway',
                elementType: 'geometry',
                stylers: {
                  color: '#004981'
                }
              },
              {
                featureType: 'highway',
                elementType: 'geometry.fill',
                stylers: {
                  color: '#005b96',
                  lightness: 1
                }
              },
              {
                featureType: 'highway',
                elementType: 'labels',
                stylers: {
                  visibility: 'off'
                }
              },
              {
                featureType: 'arterial',
                elementType: 'geometry',
                stylers: {
                  color: '#004981'
                }
              },
              {
                featureType: 'arterial',
                elementType: 'geometry.fill',
                stylers: {
                  color: '#00508b'
                }
              },
              {
                featureType: 'poi',
                elementType: 'all',
                stylers: {
                  visibility: 'off'
                }
              },
              {
                featureType: 'green',
                elementType: 'all',
                stylers: {
                  color: '#056197',
                  visibility: 'off'
                }
              },
              {
                featureType: 'subway',
                elementType: 'all',
                stylers: {
                  visibility: 'off'
                }
              },
              {
                featureType: 'manmade',
                elementType: 'all',
                stylers: {
                  visibility: 'off'
                }
              },
              {
                featureType: 'local',
                elementType: 'all',
                stylers: {
                  visibility: 'off'
                }
              },
              {
                featureType: 'arterial',
                elementType: 'labels',
                stylers: {
                  visibility: 'off'
                }
              },
              {
                featureType: 'boundary',
                elementType: 'geometry.fill',
                stylers: {
                  color: '#029fd4'
                }
              },
              {
                featureType: 'building',
                elementType: 'all',
                stylers: {
                  color: '#1a5787'
                }
              },
              {
                featureType: 'label',
                elementType: 'all',
                stylers: {
                  visibility: 'off'
                }
              }
            ]
          }
        },
        tooltip: {
          trigger: 'item'
        },
        series: [
          // 飞线开始
          {
            // name: 'wha Top2',
            // 用于带有起点和终点信息的线数据的绘制，主要用于地图上的航线，路线的可视化。
            type: 'lines',
            coordinateSystem: 'bmap',
            zlevel: 2,
            symbol: 'circle',
            symbolSize: 10,
            // symbolSize: function(val) {
            //   return 5+ val[2] * 5; //圆环大小
            // },
            // 线特效的配置
            effect: {
              show: true,
              // 特效动画的时间
              period: 8,
              // 特效尾迹的长度。取从 0 到 1 的值，数值越大尾迹越长。
              trailLength: 0,
              symbol: 'arrow',
              // 特效标记的大小，可以设置成诸如 10 这样单一的数字，也可以用数组分开表示高和宽，例如 [20, 10] 表示标记宽为20，高为10。
              symbolSize: 10
            },
            lineStyle: {
              normal: {
                color: '#a6c84c',
                width: 3,
                opacity: 0.5,
                // 边的曲度，支持从 0 到 1 的值，值越大曲度越大
                curveness: 0.2
              }
            },
            // 线数据集,需要转换数据
            data: [{
              // fromName: "北京",
              // toName: "上海",
              coords: [[102.73, 25.04], [100.97257, 22.83098]]
            },
            {
              // fromName: "上海",
              // toName: "广西",
              coords: [[100.97257, 22.83098], [103.38155, 23.37]]
            },
            {
              // fromName: "上海",
              // toName: "广西",
              coords: [[103.38155, 23.37], [104.22257, 23.40599]]
            },
            {
              // fromName: "上海",
              // toName: "广西",
              coords: [[104.22257, 23.40599], [103.72351, 27.34408]]
            },
            {
              // fromName: "上海",
              // toName: "广西",
              coords: [[103.72351, 27.34408], [98.86329, 26.89]]
            },
            {
              // fromName: "上海",
              // toName: "广西",
              coords: [[98.86329, 26.89], [100.30807, 25.68461]]
            },
            {
              // fromName: "上海",
              // toName: "广西",
              coords: [[100.30807, 25.68461], [100.385, 26.01412]]
            },
            {
              // fromName: "上海",
              // toName: "广西",
              coords: [[102.73, 25.04], [100.09544, 23.89047]]
            },
            {
              // fromName: "上海",
              // toName: "广西",
              coords: [[100.09544, 23.89047], [97.86184, 24.02397]]
            }
            ]
          },
          // 飞线结束
          {
            name: '照片数量',
            type: 'scatter',
            coordinateSystem: 'bmap',
            data: this.convertData(this.vals),
            symbolSize: function (val) {
              if (val[2] < 10) {
                return 20
              } else if (val[2] > 40) {
                return 60
              }
              return val[2] * 2
            },
            encode: {
              value: 2
            },
            label: {
              formatter: '{b}',
              position: 'right',
              show: true
            },
            itemStyle: {
              color: '#ddb926'
            },
            emphasis: {
              label: {
                show: true
              }
            }
          }
        ]
      }
      this.myMap.setOption(option)
      this.myMap.on('click', (params) => {
        if (params.target !== 'infoWindow') {
          const cityName = params.name
          this.showCityInfo(cityName)// 传入cityName显示信息
        } else {
          this.hideInfoWindow()
        }
      })
    },
    showCityInfo (name) {
      if (this.cityInfo[name]) {
        this.showInfoWindow = true
        this.cityName = name
        this.$nextTick(() => {
          this.$refs.closeBtn.addEventListener('click', this.hideInfoWindow)
        })
        // this.cityInfo = {
        //   昆明: '云南省省会,西南交通中心...',
        //   普洱: '云南省西部城市,澜沧江东岸...'
        //   // 其他城市介绍...
        // }
      }
    },
    hideInfoWindow () {
      this.showInfoWindow = false
    },
    positionInfoWindow (e) {
      const mapDOM = document.getElementById('mapEcharts')
      const clickPosition = e.event.clientX - mapDOM.offsetLeft + 'px' +
        ' ' +
        e.event.clientY - mapDOM.offsetTop + 'px'
      const position = clickPosition.split(' ')
      const mapSize = [mapDOM.clientWidth, mapDOM.clientHeight]
      const infoSize = [320, 240]

      let infoLeft = position[0] - infoSize[0] / 2
      let infoTop = position[1] - infoSize[1]

      infoLeft = Math.max(0, Math.min(infoLeft, mapSize[0] - infoSize[0]))
      infoTop = Math.max(0, Math.min(infoTop, mapSize[1] - infoSize[1]))

      this.$el.style.left = infoLeft + 'px'
      this.$el.style.top = infoTop + 'px'
    }
  },
  mounted () {
    this.$nextTick(() => {
      setTimeout(() => {
        this.initEcharts()
      }, 100)
    })
    // 点击事件中调用 positionInfoWindow 方法
    this.map.on('click', this.positionInfoWindow)
  }
}
</script>
<style scoped>
#infoWindow {
  position: absolute;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  border: 1px solid #ccc;
  background: #fff;
  padding: 20px;
  margin-top: 10px;
  min-width: 200px;
  max-height: 400px;
  overflow-y: scroll;
}

#infoTitle {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
}

#infoWindow:after {
  content: '';
  position: absolute;
  width: 0;
  height: 0;
  border: 10px solid transparent;
  border-top-color: #fff;
  top: 100%;
  left: 10px;
}
</style>

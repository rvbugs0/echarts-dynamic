<script setup>

import { ref } from "vue";
import * as echarts from 'echarts';

const charts = ref([
  {
    id: 1,
    type: "line",
    title: " Major change across the week",
    xData: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
    yData: [1, 2, 3, 4, 5, 6, 7]
  },

  {
    id: 2,
    type: "area",
    title: "Area chart",
    xData: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
    yData: [1, 2, 3, 4, 5, 6, 7]
  },
  {

    id: 3,
    title: "Trends across multiple points",
    type: "pie",
    tooltipTitle: "Access from",
    data: [{ value: 1048, name: 'Search Engine' },
    { value: 735, name: 'Direct' },
    { value: 580, name: 'Email' },
    { value: 484, name: 'Union Ads' },
    { value: 300, name: 'Video Ads' }]

  }, {
    id: 4,
    title: "Trends",
    type: "stacked-line",
    xData: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
    data: [
      {
        name: 'Email',
        type: 'line',
        stack: 'Total',
        data: [120, 132, 101, 134, 90, 230, 210]
      },
      {
        name: 'Union Ads',
        type: 'line',
        stack: 'Total',
        data: [220, 182, 191, 234, 290, 330, 310]
      },
      {
        name: 'Video Ads',
        type: 'line',
        stack: 'Total',
        data: [150, 232, 201, 154, 190, 330, 410]
      },
      {
        name: 'Direct',
        type: 'line',
        stack: 'Total',
        data: [320, 332, 301, 334, 390, 330, 320]
      },
      {
        name: 'Search Engine',
        type: 'line',
        stack: 'Total',
        data: [820, 932, 901, 934, 1290, 1330, 1320]
      }
    ]

  },
  {
    id: 5,
    title: "Bar chart",
    type: 'bar',
    xData: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
    yData: [120, 200, 150, 80, 70, 110, 130]
  }



]
)

setTimeout(() => {

  charts.value.forEach(element => {


    var chartDom = document.getElementById(element.id + '_chart');
    var myChart = echarts.init(chartDom);
    var option;


    if (element.type == "line" || element.type == "area") {

      option = {
        title: {
          left: 'center',
          text: element.title
        },
        xAxis: {
          type: 'category',
          data: element.xData
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            data: element.yData,
            type: 'line',
            smooth: true

          }
        ]
      };

      if (element.type == "area") {
        option.series.forEach(e => { e.areaStyle = {} });
      }



    } else if (element.type == "pie") {


      option = {
        title: {
          left: 'center',
          text: element.title
        },

        tooltip: {
          trigger: 'item'
        },
        legend: {
          top: '5%',
          left: 'center'
        },
        series: [
          {
            name: element.tooltipTitle,
            type: 'pie',
            radius: ['40%', '70%'],
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: 'center'
            },
            emphasis: {
              label: {
                show: true,
                fontSize: 40,
                fontWeight: 'bold'
              }
            },
            labelLine: {
              show: false
            },
            data: element.data
          }
        ]
      };




    } else if (element.type == "stacked-line") {
      option = {
        title: {
          text: element.title,
          left: 'center'
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          top: '5%',
          left: 'center',

          data: element.data.map((e) => {
            return e.name;
          })
        },
        grid: {

          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        },
        toolbox: {
          feature: {
            saveAsImage: {}
          }
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: element.xData
        },
        yAxis: {
          type: 'value'
        },
        series: element.data
      };


    } else if (element.type == "bar") {
      option = {
        title: {
          left: 'center',
          text: element.title
        },

        xAxis: {
          type: 'category',
          data: element.xData
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            data: element.yData,
            type: 'bar'
          }
        ]
      };
    }

    option && myChart.setOption(option);






  });

});




</script>



<template>
  <div id="container">
    <div class="left">
      <button>Create chart</button>



      <h1>Chart properties</h1>
      <ol>
        <li>
          Chart type - to pre-define chart options essentials
          <br>
          if Line chart : xData, yData, xTitle, yTitle
        </li>
        <li>
          API to get data
        </li>

      </ol>
    </div>
    <div class="right">

      <div v-for="chart in charts" :id="chart.id + '_chart'" style="height:500px;width: 800px;"></div>





    </div>



  </div>
</template>


<style scoped>
#container {
  display: flex;
  flex-direction: row;
  padding: 50px;
  min-height: 100vh;
  min-width: 100vw;
  background-color: grey;
  color: black;
}

.left {
  background-color: skyblue;
  left: 0px;
  width: 25%;
}

.right {
  background-color: white;
  right: 0px;
  width: 75%;
}

button {
  border: 1px solid grey;
  background-color: #4CAF50;
  /* Green */

  color: black;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}

button:hover {
  color: white;
  background-color: darkgreen;
}
</style>
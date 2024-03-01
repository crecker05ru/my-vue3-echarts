<template>
  <div class="first-chart__header">
    <h1 class="first-chart__title">Общая выручка</h1>
    <ul class="first-chart__values-list values-list">
      <li class="values-list__item">
        <h2 class="values-list__item-title">Общее оплачено</h2>
        <p class="values-list__item-value">1 123 500 &#8381;</p>
      </li>
      <li class="values-list__item">
        <h2 class="values-list__item-title"><span class="values-list__item-color" :style="{'background-color': '#9747FF;'}"></span>Деньги за мясо</h2>
        <p class="values-list__item-value">145 200 &#8381;</p>
        <div class="values-list__divider"></div>
      </li>
      <li class="values-list__item">
        <h2 class="values-list__item-title"><span class="values-list__item-color" :style="{'background-color': '#0077F7;'}"></span>Расходы на ЗП</h2>
        <p class="values-list__item-value">1 223 500 &#8381;</p>
        <div class="values-list__divider"></div>
      </li>
      <li class="values-list__item">
        <h2 class="values-list__item-title"><span class="values-list__item-color" :style="{'background-color': '#13D6FF;'}"></span>Прочее</h2>
        <p class="values-list__item-value">23 500 &#8381;</p>
        <div class="values-list__divider"></div>
      </li>
    </ul>
  </div>
 <v-chart class="first-chart__chart" :option="option" autoresize />
</template>
<script setup>
import { CanvasRenderer } from 'echarts/renderers';
import { use } from 'echarts/core';
import { BarChart, LineChart } from 'echarts/charts';
import { GridComponent, DatasetComponent,GraphicComponent,LegendComponent } from "echarts/components";
import VChart, { THEME_KEY } from 'vue-echarts';
import { ref, provide } from 'vue';


use([CanvasRenderer,BarChart, LineChart, DatasetComponent, GridComponent,GraphicComponent,LegendComponent]);

// provide(THEME_KEY, 'dark');
const color = ['#9747FF','#0077F7','#13D6FF' ];
const labels = ['Прочее','ЗП','Мясо']
const months = ['Янв', 'Фев', 'Март', 'Апр', 'Май', 'Июнь', 'Июль','Авг','Сен','Окт','Ноя','Дек']
const monthsValues = [300, 400, 200, 180, 270, 400, 500,200,180,300,280,320]
const rawData = [
  [50, 302, 301, 334, 390, 330, 320,100, 302, 301, 334, 390, 330, 320],
  [100, 132, 101, 134, 90, 230, 210,100, 302, 301, 334, 390, 330, 320],
  [140, 182, 191, 234, 290, 330, 310,100, 302, 301, 334, 390, 330, 320],
];
const maxValue = 500
const totalData = [];
for (let i = 0; i < rawData[0].length; ++i) {
  let sum = 0;
  for (let j = 0; j < rawData.length; ++j) {
    sum += rawData[j][i];
  }
  totalData.push(sum);
}
const data2 = (function () {
  let res = [];
  let len = 0;
  while (len < 12) {
    res.push(+(Math.random() * 1000 + 5).toFixed(1));
    len++;
  }
  return res;
})();
// const series = [
//     {
//       name: 'name',
//       stack: 'total',
//       barWidth: '60%',
//       data: monthsValues,
//       type: 'bar',
//       showBackground: true,
//       label: {
//       show: true,
//       formatter: (params) => `${labels[0]} %`
//     },
//     // data: 'data',
//       backgroundStyle: {
//         color: 'rgba(220, 220, 220, 0.8)'
//       }
//     }
//   ]

const series = ['Прочее','ЗП','Мясо'].map((name,index) => {
  return {
    name,
    type: 'bar',
    stack: 'total',
    barWidth: '70%',
    color: color[index],
    label: {
      show: false,
      formatter: (params) => params.value
    },
    data: rawData[index].map((val, index) =>
      totalData[index] <= 0 ? 0 : val
    )
  }
})
const grid = {
  left: 100,
  right: 100,
  top: 50,
  bottom: 50
};
const points = [[grid.left,grid.right]]
const elements = [{
  type: 'polygon',
      shape: {
        points
      },
      style: {
        fill: color[0],
        opacity: 0.25
      }
}]
const option = {
  xAxis: [{
    type: 'category',
    data: months,
    axisTick: {
        alignWithLabel: true
      },
  },
  {
      type: 'category',
      boundaryGap: true,
      data: months
    }
  ],
  yAxis: [
    {
    type: 'value',
    alignTicks: true,
      axisLine: {
        show: true,
        lineStyle: {
          color: color[0]
        }
      },
      axisLabel: {
        formatter: '{value} ml'
      }
  },
  {
      type: 'value',
      scale: true,
      name: 'Order',
      max: 1200,
      min: 0,
      boundaryGap: [0.2, 0.2]
    }
],
  series: [
    //   {
    //   name: 'Dynamic Bar',
    //   type: 'bar',
    //   xAxisIndex: 1,
    //   yAxisIndex: 1,
    //   data: series
    // },
    ...series,
    // {
    //   name: 'Dynamic Line',
    //   type: 'line',
    //   data: data2
    // }
    {
      name: 'Dynamic Line',
    type: 'line',
    xAxisIndex: 0,
    yAxisIndex: 0,
    data: data2,
    showSymbol: false,
    animationEasing: 'bounceInOut',
    animationDuration: 1000
    }
  ],
  // graphic: {
  //   elements
  // }
};
</script>
<style scoped>
.first-chart__chart {
  height: 100vh;
}
.values-list {
  display: flex;
  column-gap: 30px;
}
.values-list__item {
  position: relative;
  padding: 4px 20px;
}
.values-list__item-title {
  padding-left: 8px;
}
.values-list__item-value {
  padding-left: 8px;
}
.values-list__divider {
position: absolute;
content: '';
top: 0;
left: 0;
height: 100%;
width: 1px;
background-color: #272B33;
}
.values-list__item-color {
  position: absolute;
  display: block;
  width: 4px;
  height: 4px;
  top: 18px;
  left: 12px;
  border-radius: 50%;
  border: 1px solid transparent;
  background-color: #9747FF;
  /* background-color: #9747FF44; */
}
/* .values-list__item-color::after {
  content: "";
  position: absolute;
  width: 6px;
  height: 6px;
  top: -2px;
  left: -2px;
  border: 1px solid inherit;
  border-radius: 50%;
  background-color: #9747FF44;
} */
.values-list__item-color::before {
  content: "";
  position: absolute;
  display: block;
  width: 6px;
  height: 6px;
  top: -2px;
  left: -2px;
  border: 1px solid transparent;
  border-radius: 50%;
  background-color: inherit;
  opacity: 0.3;
}
</style>

#Chartjs 2 사용법 with React
- - -
React에서 chartjs2를 이용해 그래프를 띄우는 방법 (mix 그래프 기준)

## 1. install
```javascript
npm install react-chartjs2 --save
```
--save 는 전역이 아닌 로컬로 설치한다는 의미

## 2. import
```javascript
// use bar
import { Bar } from 'react-chartjs-2';
// use other
import { Bubble } from 'react-chartjs-2';
import { Line } from 'react-chartjs-2';
```
mix graph일 경우 Bar를 사용
line , bubble 등 다른종류면 이름을 바꿔줌  

## 3. Component setup
```javascript
<div>
  {<Bar
    data={data}
    options={Options}
  />}
</div>
```
import한 { Bar } 를 컴포넌트처럼 선언하고 data와 options를 넣는다.

## 4. input Data
```javascript
 return {
    labels: ['라벨1', '라벨2', '라벨3', '라벨4'], // x축에 들어갈 원하는 데이터를 배열형태로 넣어준다.
    datasets: [{
      label: 'label',
      type: 'line', // 원하는 그래프 형태를 의미 
      data: yData, // 마찬가지로 y축에 들어갈 원하는 데이터를 배열형태로 넣어준다.
      fill: false,
      borderColor: '#EC932F', // 색상들
      backgroundColor: '#EC932F',
      pointBorderColor: '#EC932F',
      pointBackgroundColor: '#EC932F',
      pointHoverBackgroundColor: '#EC932F',
      pointHoverBorderColor: '#EC932F',
      yAxisID: 'y-axis-2'
    }, { // 위와 동일한 형태
      label: 'label',
      type: 'bar',
      data: yData, // 마찬가지로 y축에 들어갈 원하는 데이터를 배열형태로 넣어준다.
      fill: false,
      backgroundColor: '#71B37C',
      borderColor: '#71B37C',
      hoverBackgroundColor: '#71B37C',
      hoverBorderColor: '#71B37C',
      yAxisID: 'y-axis-1'
    }]
  }
```
<Bar> 컴포넌트에 들어갈 data 형식들

## 5. input Options
```javascript
 tooltips: {
    mode: 'label'
  },
  elements: {
    line: {
      fill: false
    }
  },
  scales: {
    xAxes: [{
      display: true,
      gridLines: {
        display: false
      }
    }], // xAxes end
    yAxes: [
      { // left
        type: 'linear',
        display: true,
        position: 'left',
        id: 'y-axis-1',
        gridLines: {
          display: true
        },
        labels: {
          show: true
        },
        ticks: {
          beginAtZero: true
        }
      },
      { // right
        type: 'linear',
        display: false,
        position: 'right',
        id: 'y-axis-2',
        gridLines: {
          display: false
        },
        labels: {
          show: false
        },
        ticks: {
          beginAtZero: true
        }
      }
    ] // yAxes end
  }, // scales end
  plugins: { // 데이터 개수가 0인 경우에 이 처리를 하지 않으면 0에 그래프가 띄워져있어서 보기 안좋다.
    datalabels: {
      color: 'black',
      display: (context) => {
        return context.dataset.data[context.dataIndex] > 0;
      },
      font: {
        weight: 'bold'
      }
    }
  }
```
<Bar> 컴포넌트에 들어갈 Option 형식들


## 6. Pull code
```javascript
import React, { Component } from 'react';
import { Bar } from 'react-chartjs-2';
import ChartDataLabels from 'chartjs-plugin-datalabels'; // 그래프 중간에 값이 보이게
import allData from './timeGraphData'; // 데이터를 관리를 쉽게하려고 따로 파일을 분리
import Options from './timeGraphOptions'; // 데이터를 관리를 쉽게하려고 따로 파일을 분리

class TimeGraph extends Component {
  constructor (props) {
    super(props)
    this.state = {}
  }

  render () {
    return (
      <div>
        {<Bar
          data={allData}
          options={Options}
        />}
      </div>
    )
  }
}

export default TimeGraph;

```
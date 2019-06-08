#React-CSV 2 사용법 
- - -
React에서 csv라이브러리를 이용해 데이터를 csv 파일로 내보내는방법

## 1. install
```javascript
npm install react-csv --save;
```
--save 는 전역이 아닌 로컬로 설치한다는 의미

## 2. import
```javascript
import { CSVLink, CSVDownload } from "react-csv";
```
1. 버튼 처럼 링크로 사용하는 경우 CSVLink
2. 바로 다운로드 되게 하려면 CSVDownload

## 3. Component setup
```javascript
<div>
  <CSVLink data={data} headers={headers}>
  Export
  </CSVLink>
</div>
```
import한 { CSVLink } 를 컴포넌트처럼 선언하고 data와 headers를 넣는다.

## 4. input Data
```javascript
 headers = [
  { label: "First Name", key: "firstname" },
  { label: "Last Name", key: "lastname" },
  { label: "Email", key: "email" }
];
 
data = [
  { firstname: "Ahmed", lastname: "Tomi", email: "ah@smthing.co.com" },
  { firstname: "Raed", lastname: "Labes", email: "rl@smthing.co.com" },
  { firstname: "Yezzi", lastname: "Min l3b", email: "ymin@cocococo.com" }
];
 
<CSVLink data={data} headers={headers}>
  Download me
</CSVLink>;
```
* <CSVLink> 컴포넌트에 들어갈 data 형식들
* 나오는 형태
![csv](./img/csv.png)

## 5. Pull code
```javascript
import React, { Component } from 'react';
import { CSVLink } from 'react-csv'
import allData from './timeGraphData'; // 데이터를 관리를 쉽게하려고 따로 파일을 분리
import Options from './timeGraphOptions'; // 데이터를 관리를 쉽게하려고 따로 파일을 분리

class Export extends Component {
  constructor (props) {
    super(props)
    this.state = {}
  }

  render () {
    return (
      <div>
        {<CSVLink
          data={allData}
          options={Options}
        />}
      </div>
    )
  }
}

export default Export;

```
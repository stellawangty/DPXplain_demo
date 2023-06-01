<template>
  <div class="HomePage">
    <div class="nav">
      <div class="nav_left">
        DPXPlain
    </div>
     <!-- <div>
      <router-link to="/">Home</router-link> |
      <router-link to="/excelSearch">Database</router-link> |
      <router-link to="/averageData">Query Page</router-link> |
      <router-link to="/userMarkAverage">Explanation Page</router-link> 
    </div> -->
    </div>
    <div class="container">
        <div class="des">
          DPXPlain is the first system to the best of our knowledge for explaining group- by aggregate query answers with DP.
        </div>
    </div>
    <!-- <div class="container">
         <el-table
      :data="tableData"
      border
      style="width: 100%">
      <el-table-column
        prop="Tuple_id"
        label="Tuple_id"
        width="180">
      </el-table-column>
      <el-table-column
        prop="Time"
        label="Time"
        width="180">
      </el-table-column>
      <el-table-column
        prop="SensorID"
        label="SensorID">
      </el-table-column>
      <el-table-column
        prop="Voltage"
        label="Voltage">
      </el-table-column>
      <el-table-column
        prop="Humidity"
        label="Humidity">
      </el-table-column>
      <el-table-column
        prop="Temp"
        label="Temp">
      </el-table-column>
    </el-table>
    <el-table
      class="right"
      border
      :data="resultData"
      style="width: 100%">
      <el-table-column
        prop="ResultId"
        label="Result id"
        width="180">
      </el-table-column>
      <el-table-column
        prop="Time"
        label="Time">
      </el-table-column>
      <el-table-column
        prop="AVG"
        label="AVG(temp)">
      </el-table-column>
      <el-table-column
        prop="Label"
        label="Label">
      </el-table-column>
      <el-table-column
        prop="v"
        label="v">
      </el-table-column>
    </el-table>
    </div> -->
    <router-link to="/excelSearch" ><el-button type="primary" style="margin-top:30px">Start</el-button></router-link>
  </div>
</template>

<script>
    export default {
      data() {
        return {
          tableData: [{
            Tuple_id: 'T1',
            Time: '11AM',
            SensorID: 1,
            Voltage:2.64,
            Humidity:0.4,
            Temp:34
          },{
            Tuple_id: 'T2',
            Time: '11AM',
            SensorID: 2,
            Voltage:2.65,
            Humidity:0.5,
            Temp:34
          },{
            Tuple_id: 'T3',
            Time: '11AM',
            SensorID: 3,
            Voltage:2.63,
            Humidity:0.4,
            Temp:35
          },{
            Tuple_id: 'T4`',
            Time: '12PM',
            SensorID: 1,
            Voltage:2.7,
            Humidity:0.3,
            Temp:35
          },{
            Tuple_id: 'T5',
            Time: '11AM',
            SensorID: 2,
            Voltage:2.7,
            Humidity:0.5,
            Temp:35
          }],
          resultData:[{
            ResultId:'$1',
            Time:'11AM',
            AVG:'34.6',
            Label:'Hodl-out',
            v:'-',
          },{
            ResultId:'$2',
            Time:'12PM',
            AVG:'56.6',
            Label:'Outlier',
            v:'< -1 >',
          },{
            ResultId:'$3',
            Time:'1PM',
            AVG:'50',
            Label:'Outlier',
            v:'< -1 >',
          }]
        }
      }
    }
  </script>

<style scoped>

.HomePage{
  height: 100vh;
  width: 100vw;
  background: url(../assets/bg.png);
}
.container{
  width: 300px;
  margin: 200px auto;
}
div{
  color: #fff;
}
.nav{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 80px;
  line-height: 80px;
  color: #fff;
}
.nav .nav_left{
  margin-right: 200px;
  font-size: 20px;
}
a{
  text-decoration: none;
  color: #fff;
}
.container{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.el-table{
  /* margin: 0 auto !important;; */
  width: 50% !important;
}
.right{
  margin-left: 30px;
}
</style>
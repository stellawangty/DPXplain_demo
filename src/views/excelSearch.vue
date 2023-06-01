<template>
  <div class="containerBox">
    <div class="container">
        <div class="leftBox">
          <div class="selectBox" >
            <span v-if="value">{{value}}</span>
            <span v-else>DPXPlain</span>
            
           <el-select v-model="value" placeholder="Select Database">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
        </el-select>
        <!-- <div>
      <router-link to="/">Home</router-link> |
      <router-link to="/excelSearch">Database</router-link> |
      <router-link to="/averageData">Query Page</router-link> |
      <router-link to="/userMarkAverage">Explanation Page</router-link> 
    </div> -->
        
        </div>
        <!-- <div class="newselect">
        <el-select v-model="value" placeholder="Select Database">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
        </el-select> -->
        <el-button style="margin-left:900px" type="primary" @click="flag2=true">Confirm</el-button>
      <!-- </div> -->
        <el-table
      :data="tableData"
      border
      :default-sort = "{prop: 'Tuple_id', prop: 'marital_status ', prop: 'occupation', prop: 'education',prop: 'Humidity ',prop: 'Temp ',order: 'descending'}"
      style="width: 100%; font-size: 18px" v-if="flag2">
      <el-table-column
        sortable
        prop="Tuple_id"
        label="Tuple_id"
        width="180"
        font-weight="bold">
      </el-table-column>
      <el-table-column
        sortable
        prop="marital_status"
        label="marital_status"
        width="180">
      </el-table-column>
      <el-table-column
        sortable
        prop="occupation"
        label="occupation">
      </el-table-column>
      <el-table-column
        sortable
        prop="education"
        label="education">
      </el-table-column>
      <el-table-column
        sortable
        prop="high_income"
        label="high_income">
      </el-table-column>
        </el-table>
        </div>
    </div>
      <router-link to="/averageData" class="link">
          <el-button type="primary">Enter for query</el-button>
      </router-link>
  </div>
</template>

<script>
// import echarts from 'echarts'	// 引入echarts
import * as echarts from 'echarts';
console.log(echarts,'echarts');
    export default {
      data() {
        return {
          flag2:false,
          options:[
            {
              value:'Adult',
              label:'Adult'
            },
            {
              value:'IPUMS-CPS',
              label:'IPUMS-CPS'
            },
            {
              value:'Greman-Credit',
              label:'Greman-Credit'
            },
          ],
          value:'',
          tableData: [{
            Tuple_id: 'T1',
            marital_status : 'Never-married',
            occupation: "Machine-op-inspct",
            education:"11th",
            high_income:0
          },{
            Tuple_id: 'T2',
            marital_status : 'Married-civ-spouse',
            occupation:" Farming-fishing",
            education:"HS-grad",
            high_income:0
          },{
            Tuple_id: 'T3',
            marital_status : 'Married-civ-spouse',
            occupation: "Machine-op-inspct",
            education:"Some-college",
            high_income:1
          }],
              }
      },
      mounted(){
      },
      methods:{
      
  }   
    }
  </script>

<style scoped>
.container{
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
  width: 100%;
}
.newselect{
  display: flex;
}
.containerBox{
     padding: 100px 0 0 50px;
}
.selectBox{
  position: absolute;
  display: flex;
  align-items: center;
  right: 0;
  top: -60px;
  justify-content: space-between;
    width: 100%;
}
.leftBox{
  /* margin: 0 auto !important;; */
     width: 70%;
    margin: 0 auto;
  position: relative;
}
cell{
  /* font: 50px; */
  font-weight: bold;
  color: black
}

.link{
  margin-right: -860px;
}
button{
 margin-top: 20px;
 margin-bottom: 20px;
 /* align-items: right; */
}
/* el-table{
  font: 50px;
  font-weight: bold;
  color: black
} */

</style>
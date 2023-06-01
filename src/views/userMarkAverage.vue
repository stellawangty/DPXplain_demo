  <template>
    <div class="containerBox">
       <div class="selectBox_" >
            <span v-if="value">{{value}}</span>
            <span v-else>Adult</span>
           <!-- <el-select v-model="value" placeholder="Select Database">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
        </el-select> -->
        </div>
       <div class="container_search">
       <div style="    display: flex;
    justify-content: left;
    flex-direction: column;
    align-items: start;">
         <div class="text_weight">Initial Results</div>
           <div class="searchBox">
          SELECT marital-status, AVG(high-income) as avg-high-income FROM Adult GROUPBY marital-status
        </div>
       </div>
        <el-button style="width: 154px;height: 40px;" type="primary"  v-if="answer_hidden==false" @click="answer_hidden=true">Show True Results</el-button>
        <el-button style="width: 154px;height: 40px;" type="primary"  v-if="answer_hidden==true"  @click="answer_hidden=false">Hide True Results</el-button>
       </div>
      <div class="container">

        <el-table
        :header-cell-style="headerStyleEvent"    
          :cell-style="columnStyle"
          border
          :data="resultData"
          style="width: 100%; font-size: 16px">
          <el-table-column
            prop="group"
            label="group: marital-status"
            width="180">
          </el-table-column>
          <el-table-column
            prop="priv"
            label="Priv-answer: avg-high-income">
          </el-table-column>
          <el-table-column
            prop="answer"
            label=" True-answer
(hidden)"   v-if="answer_hidden">
          </el-table-column>
        </el-table>
        <div class="rightBox">
           <div class="OR">
            <div class="text_weight" style="font-size:20px">User question:</div>
            Why is <el-select v-model="search1" placeholder="Group">
              <el-option
                v-for="item in options1"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
          </el-select>    >
          <el-select v-model="search2" placeholder="Group">
              <el-option
                v-for="item in options1"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
          </el-select>
          ?
           </div>
          <div>
            <span>Confidence level:</span>
            <el-input v-model="Confidence" placeholder="Enter 0-1"></el-input>
          </div>
          <div class="btnBox"> <el-button @click="$router.push('/finalPage')" type="primary" >Confirm</el-button></div>
        </div>
      </div>
    </div>
  </template>

  <script>
  // import echarts from 'echarts'	// 引入echarts
  import * as echarts from 'echarts';
  console.log(echarts,'echarts');
      export default {
        data() {
          return {
            answer_hidden:false,
            search1:"",
            search2:"",
            Confidence:"",
            options:[
            {
              value:'DPXPlain',
              label:'DPXPlain'
            },
            {
              value:'Adult',
              label:'Adult'
            }
          ],
          options1:[
            {
              value:'Married-civ-spouse',
              label:'Married-civ-spouse'
            },
            {
              value:'Never-married',
              label:'Never-married'
            }
            ,
            {
              value:'Separated',
              label:'Separated'
            },
            {
              value:'Widowed',
              label:'Widowed'
            },
            {
              value:'Married-spouse-absent',
              label:'Married-spouse-absent'
            },
            {
              value:'Divorced',
              label:'Divorced'
            },
            {
              value:'Married-AF-spouse',
              label:'Married-AF-spouse'
            }
            
          ],
          value:'',
            resultData:[{
              group:'Never-married',
              priv:'0.045511',
              answer:'0.045480',
            },{
              group:'Separated',
              priv:'0.064712',
              answer:'0.064706',
            },{
              group:'Widowed',
              priv:'0.082854',
              answer:'0.084321',
            },{
              group:'Married-spouse-absent',
              priv:'0.089988',
              answer:'0.092357',
            },{
              group:'Divorced',
              priv:'0.101578',
              answer:'0.101161',
            },{
              group:'Married-AF-spouse',
              priv:'0.463193',
              answer:'0.378378',
            },{
              group:'Married-civ-spouse',
              priv:'0.446021',
              answer:'0.446133',
              test:9
            }],
          }
        },
        mounted(){
        
        },
        methods:{
           headerStyleEvent({ row, column, rowIndex, columnIndex }) {
              if (columnIndex == 2) {
                return "background:rgb(174 172 164);color:#000"
              }
            },
             columnStyle({row, column, rowIndex, columnIndex }) {
              if (columnIndex == 2 ) {
                return "background:rgb(174 172 164);";
              }

         },
        }
      }
    </script>

  <style scoped>
  .containerBox{
    position: relative;
    padding: 50px 50px 50px;
  }
  .selectBox_{
    margin-bottom: 10px;
    max-width: 40px;
  }
  .selectBox{
    top: 40px;
    
  }
  .container{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 25px 0 20px 0;
  }
  .el-table{
    /* margin: 0 auto !important;; */
    width: 70% !important;
  }

  .rightBox{
    width: 35%;
    height: 200px;
    margin: 0 auto;

    padding-left:50px;
    text-align: left;
  }
    .rightBox >div span{
      margin-top: 5px;
    }
     .rightBox .el-input{
      width: 230px;
      margin-left:15px;
     }
    .rightBox .el-select{
    width: 165px;
    margin-left: 5px;
  }
  .rightBox .OR{
    margin: 15px 0;
    display: block;
  }
  .container_search{
    display: flex;
    justify-content: space-between;
     width: 65%;
     align-items: center;
  }
  .searchBox {
       /* width: 43%; */
    text-align: left;
    
  }
  .searchBox .el-select{
    margin:0px  10px;
    width: 120px;
  }
  .btnBox{
    margin-top: 20px;
  }
  .text_weight{
    font-size: 18x;
    font-weight: 700;
    margin-bottom: 15px;
  }
  </style>
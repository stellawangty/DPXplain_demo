  <template>
    <div class="containerBox">
      <div class="descriptionBox" v-if="flag1">
        <div class="title">Initial Explanation</div>
        <div>The 95% confidence interval of predicatedifference is (0.399, 0.402), hence the noise inthe query is possibly not the reason.</div>
        <div>
          Proceed to predicate explanations
          <el-button style="margin-left:30px" type="primary" @click="flag2=true">Confirm</el-button>
        </div>
      </div>
       <div class="selectBox_" v-if="flag2">
           <el-select v-model="value" placeholder="Choose top-k">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
        </el-select>
         <el-button style="margin-left:30px" type="primary" @click="flag3=true">Confirm</el-button>
        </div>
       <div class="container_search" v-if="flag3">
           <div>
            Please enter privacy budget for explanations: 
            </div>
            <el-input v-model="val1" placeholder="predicate"></el-input>
            <el-input v-model="val2" placeholder="influence"></el-input>
            <el-input v-model="val3" placeholder="rank"></el-input>
            <el-button type="primary" @click="clickMe">Confirm</el-button>
       </div>
      <div class="container" v-if="flag4">
        <el-table
        class="finallPage"
         :header-cell-style="handerMethod"
          border
          :data="resultData"
          style="width: 100%">
          <el-table-column
            prop="predicate"
            label="explanation predicate"
            width="260">
          </el-table-column>
          <el-table-column
            prop="Influ_L"
            label="Rel Influ 95%-CI (  L / U)">
          </el-table-column>
           <el-table-column
            prop="Influ_U"
            label="Rel Influ 95%-CI (  L / U)">
          </el-table-column>
          <el-table-column
            prop="Rank_L"
            label="Rank 95%-CI (  L / U)">
          </el-table-column>
           <el-table-column
            prop="Rank_U"
            label="Rank 95%-CI (  L / U)">
          </el-table-column>
        </el-table>
        <div class="btnBox" style="    display: flex;justify-content: end;">
           <el-button @click="$router.push('/userMarkAverage')" type="info">Back to Question</el-button>
          <el-button  @click="$router.push('/excelSearch')" style="margin-left:30px" type="primary">Back to Query</el-button> 
       </div>
       <div class="influence">
        
          <div class="con">
            <el-button  type="info" @click="test11">Back to Question</el-button>
              <div id="echart1" style="width: 700px;height: 600px;margin:50px auto"></div>
          </div>
       </div>
       <div class="rank">
          <div class="con">
            <div id="echart2" style="width: 700px;height: 600px;margin:50px auto"></div>
          </div>
       </div>
       <div style="display:flex;justify-content: end;margin-top:50px" @click="clickMe2"><el-button type="primary">examine stability of the explanation</el-button></div>
      </div>
      <div class="bottomBox" v-if="flag5">
        <div class="con">
            <div class="echart" id="mychart2" :style="myChartStyle"></div>
        </div>
        Change privacy budget for top-k predicate explanations and see the generated results?
         <div style="display:flex;justify-content: end"><el-button type="primary">Confrim</el-button></div>
      </div>
    </div>
  </template>

  <script>
    var xData1 = [[2.76, 'age'], [6.5, 'age'], [9.63, 'age']]
  var xData2 = [[2.93, 'education'], [5.5, 'education'], [9.8, 'education']]
  var xData3 = [[3.25, 'occupation'], [7.5, 'occupation'], [10.12, 'occupation']]
  var yData = ['age', 'education', 'occupation']

  var xData1_1 = [[1, 'age'], [6.3, 'age'], [8, 'age']]
  var xData2_1 = [[1, 'education'], [5, 'education'], [8, 'education']]
  var xData3_1 = [[1, 'occupation'], [6, 'occupation'], [8, 'occupation']]
  // import echarts from 'echarts'	// 引入echarts
  import * as echarts from 'echarts';
import { set } from 'vue'
  console.log(echarts,'echarts');
      export default {
        data() {
          return {
            flag1:true,
            flag2:false,
            flag3:false,
            flag4:false,
            flag5:false,
            val3:'',
            val2:'',
            val1:'',
            search1:"",
            search2:"",
            Confidence:"",
            option_1: {
        title:{
          text:"Visualization: influence"
        },
        legend:{
          data: ["EXEC","education","age"]
        },
        
          yAxis: {
            type: 'category',
            data: yData
          },
          xAxis: {
            type: 'value',
            axisLine:{show:true},
          },
          series: [
            {
              data: xData1,
              type: 'line',
               markPoint: {
                 label: {
                color: '#FFF',  // 文字颜色
                show: true,
                fontSize:12,
                formatter: 'True value'  // 自定义文字内容
              },
              data: [{ type: 'average' }],
               symbol: "square",
               symbolSize: [60 ,30], 
              symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
               },
               itemStyle:{
                  normal:{
                      label: {
                        show: true,
                        position: 'top'
                      },
                  },
               }
            },
            {
              data: xData2,
              type: 'line',
               markPoint: {
                 label: {
                color: '#FFF',  // 文字颜色
                show: true,
                fontSize:12,
                formatter: 'True value'  // 自定义文字内容
              },
              data: [{ type: 'average' }],
               symbol: "square",
               symbolSize: [60 ,30], 
              symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
               },
              itemStyle:{
                  normal:{
                      label: {
                        show: true,
                        position: 'top'
                      },
                  },
               }
            },
            {
              data: xData3,
              type: 'line',
               markPoint: {
                 label: {
                color: '#FFF',  // 文字颜色
                show: true,
                fontSize:12,
                formatter: 'True value'  // 自定义文字内容
              },
              data: [{ type: 'average' }],
               symbol: "square",
               symbolSize: [60 ,30], 
              symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
               },
              symbolSize:3,//一定要加这个字段才能显示
              itemStyle:{
                  normal:{
                      label: {
                        show: true,
                        position: 'top'
                      },
                  },
               },
               
            },
            
          ]
          
        },
            options:[
            {
              value:'db1',
              label:'db1'
            },
            {
              value:'db2',
              label:'db2'
            }
          ],
          value:'',
            resultData:[{
              predicate:'occupation =“Exec-managerial"',
              Influ_L:'3.25%',
              Influ_U:'10.12%',
              answer:'0.045480',
              Rank_L:1,
              Rank_U:9
            },{
              predicate:'education =“Bachelors"',
              Influ_L:'2.93%',
              Influ_U:'9.80%',
              answer:'0.064706',
              Rank_L:1,
              Rank_U:8
            },{
              predicate:'age="(40.501)"',
              Influ_L:'2.76%',
              Influ_U:'9.637',
              answer:'0.084321',
               Rank_L:1,
              Rank_U:8
            },{
              predicate:'occupation ="prof-specialty"',
              Influ_L:'0.94%',
              Influ_U:'7.81%',
              answer:'0.092357',
               Rank_L:1,
              Rank_U:18
            },{
              predicate:'relationship =“Own-child"',
              Influ_L:'-0.49%6',
              Influ_U:'9.80%',
              answer:'6.38',
               Rank_L:1,
              Rank_U:96,
            }],
            xData: [1,2,3,4,5], //横坐标
            yData: [1.5,3,3,3,3], //人数数据
            myChartStyle: { float: "left", width: "100%", height: "400px" } //图表样式
          }
        },
        mounted(){
        },
        methods:{
          clickMe(){
            let _this=this
          this.flag4=true
            setTimeout(
                function(props){
                    _this.initEcharts2()
                    _this.initEcharts1()
                },1000,
            )
          },
           clickMe2(){
            let _this=this
            this.flag5=true
            setTimeout(
                function(props){
                    _this.initEcharts()
                },1000,
            )
          },
         
          getSelection(){
          },
           handerMethod ({ row, column, rowIndex, columnIndex }) {
            console.log( row,columnIndex)
      if (row[0].level == 1) {
        //这里有个非常坑的bug 必须是row[0]=0 row[1]=2才会生效
        row[1].colSpan = 0
        row[3].colSpan =0
        row[3].colSpan =2
        row[2].colSpan = 2
        if (columnIndex === 1) {
          return { display: 'none' }
        }
       
         if (columnIndex === 4) {
          return { display: 'none' }
        }
      }
    },
           initEcharts() {
          const option = {
            xAxis: {
              data: this.xData,
              name:'Top-kPrivacyBudget',
               axisTick:{
                show:false
               }
            },
            yAxis: {
               name:'Change inPredicate Set',
                axisTick:{
                show:false
               },
               axisLine:{
                show:true
               }
            },
            series: [
              {
                data: this.yData,
                type: "line", // 类型设置为折线图,
                  label: {
                color: '#FFF',  // 文字颜色
                show: true,
                fontSize:12,
                formatter: '9999'  // 自定义文字内容
                 },
              itemStyle:{
                  normal:{
                      label: {
                        show: true,
                        position: 'top'
                      },
                  },
               },
                markPoint: {
                label: {
                color: '#FFF',  // 文字颜色
                show: true,
                fontSize:12,
                formatter: 'here is [1,1.5]'  // 自定义文字内容
              },
              data: [{ type: 'average' } ],
                symbol: "arrow",
                symbolSize: [80,50], 
                symbolOffset: ['-180px', '80px'],  // 调整标注图片的位移方向 大小
                symbol: 'path://m 0,0 h 48 v 20 h -30 l -6,10 l -6,-10 h -6 z', 
               },
              }
            ]
          };
          this.myChart = echarts.init(document.getElementById("mychart2"));
          this.myChart.setOption(option);
          //随着屏幕大小调节图表
          window.addEventListener("resize", () => {
            this.myChart.resize();
          });
    },
    
     //柱状图啊
      initEcharts1(){
         // 初始化 echarts 实例
        var echart = echarts.init(document.getElementById('echart1'))
        // 配置图表所需的数据
        // 调用 setOption 方法绘制图表
        echart.setOption(this.option_1)
      },
        test11(){
        console.log(this.option_1.series[0].markPoint.label.show)
        console.log(this.option_1)
        this.option_1.series.forEach(item=>{
          item.markPoint.label.show=false
          item.markPoint.symbolSize=0
        })
        this.initEcharts1()
      },
       //折线图2--rank
      initEcharts2(){
         // 初始化 echarts 实例
        var echart = echarts.init(document.getElementById('echart2'))
        // 配置图表所需的数据
       var option = {
        title:{
          text:"Visualization: rank"
        },
        legend:{
          data: ["EXEC","education","age"]
        },
        
          yAxis: {
            type: 'category',
            data: yData
          },
          xAxis: {
            type: 'value',
            axisLine:{show:true},
          },
          
          series: [
            {
              data: xData1_1,
              type: 'line',
               markPoint: {
                 label: {
                color: '#FFF',  // 文字颜色
                show: true,
                fontSize:12,
                formatter: 'True value'  // 自定义文字内容
              },
              data: [{ type: 'average' }],
               symbol: "square",
               symbolSize: [60 ,30], 
              symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
               },
               itemStyle:{
                  normal:{
                      label: {
                        show: true,
                        position: 'top'
                      },
                  },
               }
            },
            {
              data: xData2_1,
              type: 'line',
               markPoint: {
                 label: {
                color: '#FFF',  // 文字颜色
                show: true,
                fontSize:12,
                formatter: 'True value'  // 自定义文字内容
              },
              data: [{ type: 'average' }],
               symbol: "square",
               symbolSize: [60 ,30], 
              symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
               },
              itemStyle:{
                  normal:{
                      label: {
                        show: true,
                        position: 'top'
                      },
                  },
               }
            },
            {
              data: xData3_1,
               markPoint: {
                 label: {
                color: '#FFF',  // 文字颜色
                show: true,
                fontSize:12,
                formatter: 'True value'  // 自定义文字内容
              },
              data: [{ type: 'average' }],
               symbol: "square",
               symbolSize: [60 ,30], 
              symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
               },
              type: 'line',
              itemStyle:{
                  normal:{
                      label: {
                        show: true,
                        position: 'top'
                      },
                  },
               }
            },
            
          ]
          
        };
        // 调用 setOption 方法绘制图表
        echart.setOption(option)
      }
        }
      }
    </script>

  <style scoped>
  .containerBox{
    position: relative;
    padding: 50px 50px 50px;
  }

  .descriptionBox{
    width: 370px;
    margin: 0 auto;
    text-align: left;
  }
  .descriptionBox .title{
    text-align: center;
    font-size: 16px;
    font-weight: 700;
  }
   .descriptionBox div{
    margin-bottom: 20px;
   }
  .selectBox{
    top: 40px;
  }
  .container{
    margin: 25px 0 20px 0;
  }
  .selectBox_{
    display: flex;
    justify-content: flex-start;
    margin-bottom: 10px ;
  }
  .el-table{
    /* margin: 0 auto !important;; */
  }
    .rightBox >div span{
      margin-top: 5px;
    }
     .rightBox .el-input{
      width: 230px;
      margin-left:15px;
     }
      .rightBox .el-select{
    width: 100px;
    margin-left: 5px;
  }
  .rightBox .OR{
    margin: 15px 0;
    display: block;
  }
  .container_search{
    display: flex;
    align-items: center;
  }
   .container_search .el-input{
    width: 100px;
    margin: 0 8px;
   }
  .searchBox {
       width: 43%;
    text-align: left;
  }
  .searchBox .el-select{
    margin:0px  10px;
    width: 120px;
  }
  .btnBox{
    margin-top: 20px;
  }
  .con{
    height: 500px;
  }
  .influence{
     margin-bottom: 100px;
  }
  
  </style>

  <style >
    .finallPage.el-table th.el-table__cell>.cell{
    text-align: center !important;
  }
  </style>
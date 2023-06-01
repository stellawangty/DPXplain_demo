  <template>
    <div class="containerBox">
      <div class="descriptionBox" v-if="flag1">
        <div class="qa">
          <div class="q">
            <div class="title">Question</div>
            <div>Why avg-high-income of group "Married-civ-spouse" > that of group "Never-married"?</div>
          </div>
          <div class="a">
            <div class="title">Initial Explanation</div>
            <div>The 95% confidence interval of predicate difference is (0.399, 0.402), hence the noise in the query is possibly not the reason.</div>
          </div>
        </div>
        
          <div>
          Proceed to predicate explanations?
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
        <span class="toolstips">
          <el-tooltip class="item" effect="dark" content="The k best answers" placement="top-start">
          <img src="../assets/wen.png" alt="">
        </el-tooltip> 
        </span>
         <el-button style="margin-left:30px" type="primary" @click="flag3=true">Confirm</el-button>
        </div>
       <div class="container_search" v-if="flag3">
           <div>
            Please enter privacy budget for explanations: 
            </div>
            <el-input v-model="val1" placeholder="predicate"></el-input>
            <span class="toolstips">
                <el-tooltip class="item" effect="dark" content="The privacy budget to privately find the top-k explanation predicates." placement="top-start">
                  <img src="../assets/wen.png" alt="">
                </el-tooltip> 
            </span>
            <el-input v-model="val2" placeholder="influence"></el-input>
            <span class="toolstips">
                <el-tooltip class="item" effect="dark" content="The privacy budget to privately find the 95%-level confidence intervals of the predicate influence." placement="top-start">
                  <img src="../assets/wen.png" alt="">
                </el-tooltip> 
            </span>
            <el-input v-model="val3" placeholder="rank"></el-input>
             <span class="toolstips">
                <el-tooltip class="item" effect="dark" content="The privacy budget to privately find the 95%-level confidence intervals of the predicate rank." placement="top-start">
                  <img src="../assets/wen.png" alt="">
                </el-tooltip> 
            </span>
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
            label="Rel Influ 95%-CI (L / U)">
          </el-table-column>
           <el-table-column
            prop="Influ_U"
            label="Rel Influ 95%-CI (L / U)">
          </el-table-column>
          <el-table-column
            prop="Rank_L"
            label="Rank 95%-CI (L / U)">
          </el-table-column>
           <el-table-column
            prop="Rank_U"
            label="Rank 95%-CI (L / U)">
          </el-table-column>
        </el-table>
        <div class="btnBox" style="     display: flex;justify-content: end;">
           <el-button @click="$router.push('/userMarkAverage')" type="primary">Back to Question</el-button>
          <el-button  @click="$router.push('/excelSearch')" style="margin-left:30px" type="primary">Back to Query</el-button> 
       </div>
       <div class="influence">
          <div class="con" style="position:relative;width: 1200px;margin: 0 auto;">
              <el-button  style="z-index: 99999999999999999999;position: absolute;top: -6px;left: 230px;font-size: 18px;}" type="primary" @click="test11">Hide True Value</el-button>
              <div id="echart1" style="width: 1200px;height: 600px;margin:50px auto"></div>
          </div>
       </div>
       <div class="rank">
        <div class="con" style="position:relative;width: 1200px;margin: 0 auto;">
            <el-button  style="z-index: 99999999999999999999;position: absolute;top: -6px;left: 230px;font-size: 18px;}" type="primary" @click="test12">Hide True Value</el-button>
          <div id="echart2" style="width: 1200px;height: 600px;margin:50px auto"></div>
        </div>
     </div>
       <div class="influence1">
          <div class="con">
              <div id="echart3" style="width: 1200px;height: 600px;margin:50px auto"></div>
          </div>
       </div>
       <div class="rank1">
          <div class="con">
            <div id="echart4" style="width: 1200px;height: 600px;margin:50px auto"></div>
          </div>
       </div>
       <!-- <div style="display:flex;justify-content: end;margin-top:50px" @click="clickMe3"><el-button type="primary">Hide True values</el-button></div> -->
       <!-- <div style="display:flex;justify-content: end;margin-top:50px" @click="clickMe2"><el-button type="primary">Examine Stability of the Explanation</el-button></div> -->
      </div>
      <!-- <div class="bottomBox" v-if="flag5">
        <div class="con">
            <div class="echart" id="mychart2" :style="myChartStyle"></div>
        </div>
        Change privacy budget for top-k predicate explanations and see the generated results?
         <div style="display:flex;justify-content: end"><el-button type="primary">Confrim</el-button></div>
      </div> -->


    </div>
  </template>

<script>
var xData1 = [[-0.49, 'relationship = \"Own-child\"'], [4.5, 'relationship = \"Own-child\"'], [6.38, 'relationship = \"Own-child\"']]
var xData2 = [[0.94, 'occupation = \"prof-specialty\"'], [5.4, 'occupation = \"prof-specialty\"'], [7.81, 'occupation = \"prof-specialty\"']]
var xData3 = [[2.76, 'age = \"(40,50]\"'], [6.5, 'age = \"(40,50]\"'], [9.63, 'age = \"(40,50]\"']]
var xData4 = [[2.93, 'education = \"Bachelors\"'], [5.5, 'education = \"Bachelors\"'], [9.8, 'education = \"Bachelors\"']]
var xData5 = [[3.25, 'occupation = \"Exec\"'], [7.5, 'occupation = \"Exec\"'], [10.12, 'occupation = \"Exec\"']]
var yData = ['relationship = \"Own-child\"', 'occupation = \"prof-specialty\"', 'age = \"(40,50]\"', 'education = \"Bachelors\"', 'occupation = \"Exec\"']

var xData1_1 = [[1, 'relationship = \"Own-child\"'], [45, 'relationship = \"Own-child\"'], [96, 'relationship = \"Own-child\"']]
var xData2_1 = [[1, 'occupation = \"prof-specialty\"'], [9, 'occupation = \"prof-specialty\"'], [18, 'occupation = \"prof-specialty\"']]
var xData3_1 = [[1, 'age = \"(40,50]\"'], [6, 'age = \"(40,50]\"'], [8, 'age = \"(40,50]\"']]
var xData4_1 = [[1, 'education = \"Bachelors\"'], [5, 'education = \"Bachelors\"'], [8, 'education = \"Bachelors\"']]
var xData5_1 = [[1, 'occupation = \"Exec\"'], [6, 'occupation = \"Exec\"'], [9, 'occupation = \"Exec\"']]

var xData1_2 = [[4.3, 'top-1'], [5, 'top-1']]
var xData2_2 = [[2.5, 'top-2'], [4.3, 'top-2']]
var xData3_2 = [[1.5, 'top-3'], [2.5, 'top-3']]
var xData4_2 = [[2, 'top-4']]
var xData5_2 = [[1.5, 'top-5']]
var xData6_2 = [[1, 'top-6']]
var yData_2 = ['top-1', 'top-2', 'top-3', 'top-4', 'top-5', 'top-6']

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
          options:[
          {
            value:'1',
            label:'1'
          },
          {
            value:'2',
            label:'2'
          },
          {
            value:'3',
            label:'3'
          },
          {
            value:'4',
            label:'4'
          },
          {
            value:'5',
            label:'5'
          },
        ],
            option_1: {
      title:{
        text:"Visualization: influence",
        fontSize: 25
      },
      legend:{
        data: ["EXEC","education","age"]
      },
      
        yAxis: {
          type: 'category',
          data: yData
        },
        xAxis: {
          name: 'Influence',
          type: 'value',
          axisLine:{show:true},
        },
        series: [
        {
            data: xData1,
            type: 'line',
            symbolSize: 15,
            // symbol: triangle,
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:18,
              formatter: 'True value', // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [100 ,40], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
             },
             itemStyle:{
                normal:{
                    label: {
                      show: true,
                      position: 'top',
                      textStyle:{
                        fontSize: 15
                      }
                    },
                },
             }
          },
          {
            data: xData2,
            type: 'line',
            symbolSize: 15,
            // symbol: triangle,
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:18,
              formatter: 'True value', // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [100 ,40], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
             },
             itemStyle:{
                normal:{
                    label: {
                      show: true,
                      position: 'top',
                      textStyle:{
                        fontSize: 15
                      }
                    },
                },
             }
          },
          {
            data: xData3,
            type: 'line',
            symbolSize: 15,
            // symbol: triangle,
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:18,
              formatter: 'True value', // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [100 ,40], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
             },
             itemStyle:{
                normal:{
                    label: {
                      show: true,
                      position: 'top',
                      textStyle:{
                        fontSize: 15
                      }
                    },
                },
             }
          },
          {
            data: xData4,
            type: 'line',
            symbolSize: 15,
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:18,
              formatter: 'True value'  // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [100 ,40], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
             },
            itemStyle:{
                normal:{
                    label: {
                      show: true,
                      position: 'top',
                      textStyle:{
                        fontSize: 15
                      }
                    },
                },
             }
          },
          {
            data: xData5,
            type: 'line',
            symbolSize: 15,
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:18,
              formatter: 'True value'  // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [100 ,40], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
             },
            itemStyle:{
                normal:{
                    label: {
                      show: true,
                      position: 'top',
                      textStyle:{
                        fontSize: 15
                      }
                    },
                },
             },
             
          },
          
        ]
        
      },

      option_2: {
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
          name: 'Rank',
          type: 'value',
          axisLine:{show:true},
        },
        series: [
        {
            data: xData1_1,
            type: 'line',
            symbolSize: 15,
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:18,
              formatter: 'True value'  // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [100 ,40], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
             },
             itemStyle:{
                normal:{
                    label: {
                      show: true,
                      position: 'top',
                      textStyle:{
                        fontSize: 15
                      }
                    },
                },
             }
          },
          {
            data: xData2_1,
            type: 'line',
            symbolSize: 15,
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:18,
              formatter: 'True value'  // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [100 ,40], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
             },
             itemStyle:{
                normal:{
                    label: {
                      show: true,
                      position: 'top',
                      textStyle:{
                        fontSize: 15
                      }
                    },
                },
             }
          },
          {
            data: xData3_1,
            type: 'line',
            symbolSize: 15,
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:18,
              formatter: 'True value'  // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [100 ,40], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
             },
             itemStyle:{
                normal:{
                    label: {
                      show: true,
                      position: 'top',
                      textStyle:{
                        fontSize: 15
                      }
                    },
                },
             }
          },
          {
            data: xData4_1,
            type: 'line',
            symbolSize: 15,
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:18,
              formatter: 'True value'  // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [100 ,40], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
             },
            itemStyle:{
                normal:{
                    label: {
                      show: true,
                      position: 'top',
                      textStyle:{
                        fontSize: 15
                      }
                    },
                },
             }
          },
          {
            data: xData5_1,
            type: 'line',
            symbolSize: 15,
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:18,
              formatter: 'True value'  // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [100 ,40], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
             },
            itemStyle:{
                normal:{
                    label: {
                      show: true,
                      position: 'top',
                      textStyle:{
                        fontSize: 15
                      }
                    },
                },
             },
             
          },
          
        ]
        
      },















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
            predicate:'age="(40,50]"',
            Influ_L:'2.76%',
            Influ_U:'9.63%',
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
            Influ_L:'-0.49%',
            Influ_U:'6.38%',
            answer:'6.38',
             Rank_L:1,
            Rank_U:96,
          }],
          xData: [1,2,3,4,5], 
          yData: [1.5,3,3,3,3],
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
                  // _this.initEcharts3()
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
        // clickMe3(){
        //   let _this=this
        //   this.flag5=true
        //   setTimeout(
        //       function(props){
        //           _this.initEcharts()
        //       },1000,
        //   )
        // },
       
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
            name:'Top-k Privacy Budget',
             axisTick:{
              show:false
             }
          },
          yAxis: {
             name:'Change in Predicate Set',
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
              color: '#00FF00',  // 文字颜色
              show: true,
              fontSize:12,
              formatter: 'You are here!'  // 自定义文字内容
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

    initEcharts2(){
       // 初始化 echarts 实例
      var echart = echarts.init(document.getElementById('echart2'))
      // 配置图表所需的数据
      // 调用 setOption 方法绘制图表
      echart.setOption(this.option_2)
    },
      test12(){
      console.log(this.option_2.series[0].markPoint.label.show)
      console.log(this.option_2)
      this.option_2.series.forEach(item=>{
        item.markPoint.label.show=false
        item.markPoint.symbolSize=0
      })
      this.initEcharts2()
    },
    
     //折线图2--rank
    // initEcharts2(){
    //    // 初始化 echarts 实例
    //   var echart = echarts.init(document.getElementById('echart2'))
    //   // 配置图表所需的数据
    //  var option = {
    //   title:{
    //     text:"Visualization: rank"
    //   },
    //   legend:{
    //     data: ["EXEC","education","age"]
    //   },
      
    //     yAxis: {
    //       type: 'category',
    //       data: yData
    //     },
    //     xAxis: {
    //       type: 'value',
    //       axisLine:{show:true},
    //     },
        
    //     series: [
    //       {
    //         data: xData1_1,
    //         type: 'line',
    //          markPoint: {
    //            label: {
    //           color: '#FFF',  // 文字颜色
    //           show: true,
    //           fontSize:12,
    //           formatter: 'True value'  // 自定义文字内容
    //         },
    //         data: [{ type: 'average' }],
    //          symbol: "square",
    //          symbolSize: [60 ,30], 
    //         symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
    //          },
    //          itemStyle:{
    //             normal:{
    //                 label: {
    //                   show: true,
    //                   position: 'top'
    //                 },
    //             },
    //          }
    //       },
    //       {
    //         data: xData2_1,
    //         type: 'line',
    //          markPoint: {
    //            label: {
    //           color: '#FFF',  // 文字颜色
    //           show: true,
    //           fontSize:12,
    //           formatter: 'True value'  // 自定义文字内容
    //         },
    //         data: [{ type: 'average' }],
    //          symbol: "square",
    //          symbolSize: [60 ,30], 
    //         symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
    //          },
    //         itemStyle:{
    //             normal:{
    //                 label: {
    //                   show: true,
    //                   position: 'top'
    //                 },
    //             },
    //          }
    //       },
    //       {
    //         data: xData3_1,
    //          markPoint: {
    //            label: {
    //           color: '#FFF',  // 文字颜色
    //           show: true,
    //           fontSize:12,
    //           formatter: 'True value'  // 自定义文字内容
    //         },
    //         data: [{ type: 'average' }],
    //          symbol: "square",
    //          symbolSize: [60 ,30], 
    //         symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
    //          },
    //         type: 'line',
    //         itemStyle:{
    //             normal:{
    //                 label: {
    //                   show: true,
    //                   position: 'top'
    //                 },
    //             },
    //          }
    //       },
          
    //     ]
        
    //   };
    //   // 调用 setOption 方法绘制图表
    //   echart.setOption(option)
    // },







    initEcharts3(){
       // 初始化 echarts 实例
      var echart = echarts.init(document.getElementById('echart3'))
      // 配置图表所需的数据
     var option = {
      title:{
        text:"Visualization: Top-Z"
      },
      legend:{
        data: ["EXEC","education","age"]
      },
      
        yAxis: {
          type: 'category',
          data: yData_2
        },
        xAxis: {
          name: 'Relative Influence',
          type: 'value',
          axisLine:{show:true},
        },
        
        series: [
          {
            data: xData1_2,
            type: 'line',
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:12,
              formatter: 'True Top-1', // 自定义文字内容
              // symbol: triangle
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [60 ,30], 
            symbolOffset: [0, '-50px']  // 调整标注图片的位移方向 大小
            
             },
            //  markPoint: {
            //    label: {
            //   color: '#FFF',  // 文字颜色
            //   show: true,
            //   fontSize:12,
            //   formatter: 'Private Top-Z'  // 自定义文字内容
            // },
            // data: [{ type: 'average' }],
            //  symbol: "square",
            //  symbolSize: [60 ,30], 
            // symbolOffset: ['-50px', '-50px']  // 调整标注图片的位移方向 大小
            //  },
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
            data: xData2_2,
            type: 'line',
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:12,
              formatter: 'True Top-2'  // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [60 ,30], 
            symbolOffset: ['350px', '-50px']  // 调整标注图片的位移方向 大小
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
            data: xData3_2,
            type: 'line',
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:12,
              formatter: 'True Top-3'  // 自定义文字内容
            },
            data: [{ type: 'average' }],
             symbol: "square",
             symbolSize: [60 ,30], 
            symbolOffset: ['200px', '-50px']  // 调整标注图片的位移方向 大小
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
          {
            data: xData4_2,
            type: 'line',
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:12,
              formatter: 'True Top-4'  // 自定义文字内容
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
            data: xData5_2,
            type: 'line',
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:12,
              formatter: 'True Top-5'  // 自定义文字内容
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
          {
            data: xData6_2,
            type: 'line',
             markPoint: {
               label: {
              color: '#FFF',  // 文字颜色
              show: true,
              fontSize:12,
              formatter: 'True Top-6'  // 自定义文字内容
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
  width: 550px;
  margin: 0 auto;
  text-align: left;
}
.descriptionBox .title{
  text-align: center;
  font-size: 16px;
  font-weight: 700;
}
 .descriptionBox div{
  margin-bottom: 10px;
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
.qa{
  display: flex;
  align-items: center;
  justify-content: space-between;
 }
 .q{
  margin-right: 15px;
 }
 .a{
  margin-left: 15px;
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
.toolstips{
    margin-top: -30px;
    margin-left: -20px;
    z-index: 10000;
    cursor: pointer;
  }
  </style>
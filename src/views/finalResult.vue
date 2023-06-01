  <template>
    <div class="containerBox">
       <div class="selectBox_" >
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
        </div>
       <div class="container_search">
           <div class="searchBox">
          SELECT
          <div class="selectBox1" style="position:relative;display:inline-block">
            <!-- <i class="el-icon-plus plus1" @click="dialogVisible=true"></i> -->
           <el-select v-model="search1" placeholder="Group">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
          </el-select>
          </div>
         AS
          <div class="selectBox1" style="position:relative;display:inline-block">
          <!-- <i class="el-icon-plus plus2" @click="dialogVisible=true"></i> -->
         <el-select v-model="search2" placeholder="ATT">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
          </el-select>
          </div>
          FROM
          <div class="selectBox1" style="position:relative;display:inline-block">
          <!-- <i class="el-icon-plus plus2" @click="dialogVisible=true"></i> -->
         <el-select v-model="search2" placeholder="DB">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
          </el-select>
          </div>
          AS
          <div class="selectBox1" style="position:relative;display:inline-block">
          <!-- <i class="el-icon-plus plus2" @click="dialogVisible=true"></i> -->
         <el-select v-model="search2" placeholder="ATT">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
          </el-select>
          </div>
          WHERE
          <div class="selectBox1" style="position:relative;display:inline-block">
          <!-- <i class="el-icon-plus plus2" @click="dialogVisible=true"></i> -->
         <el-select v-model="search2" placeholder="Group">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
          </el-select>
          </div>
          GROUP BY
          <div class="selectBox1" style="position:relative;display:inline-block">
            <el-select v-model="search3" placeholder="ATT">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
              
          </el-select>
            <!-- <i class="el-icon-plus plus3" @click="dialogVisible=true"></i> -->
          </div>
          
        </div>
        <div class="plusAd">
             
           
           
        </div>
       </div>
      <div class="container">

        <el-table
          border
          :data="resultData"
          style="width: 100%">
          <el-table-column
            prop="Tuple_id"
            label="Tuple_id"
            width="180">
          </el-table-column>
          <el-table-column
            prop="marital_status"
            label="marital_status">
          </el-table-column>
          <el-table-column
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
        <div class="rightBox">
          <div>
             <span>Enter a privacy budget:</span><br>
            <el-input v-model="budgetNum"></el-input>
          </div>
          <div class="OR">OR</div>
          <div>
             <el-button style="margin-right:10px" type="primary" @click="rangeRandom(1,5)">Choose a budget for me</el-button>
            <span style="display:inline-block;" v-if="chooseNum!==''">{{chooseNum}}</span>
            <span  style="display:inline-block;" v-else>budget</span>
            <br>
            <span style="color:#ccc">*A random number in [O.1-5]</span>
          </div>
        </div>
       
      </div>
       <router-link to="/userMarkAverage"><el-button style="margin-right:-340px" type="primary" >Run query</el-button></router-link>
        <el-dialog
        title="Aggregate Query template"
        :visible.sync="dialogVisible"
        width="40%">
        <div>
            <div class="left" >
              <div class="con" v-for="item in 4" :key="item">{{item}}</div>
            </div>
           <div class="right">
               <div>SELECT <el-input v-model="dia.search1"></el-input>AS<el-input class="AS" v-model="dia.search2"></el-input></div>
                <div> FROM <el-input v-model="dia.search3"></el-input>AS<el-input class="AS" v-model="dia.search4"></el-input></div>
                <div>WHERE <el-input v-model="dia.search5"></el-input></div>
                <div>GROU PBY <el-input v-model="dia.search6"></el-input></div>
           </div>
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="dialogVisible = false">Run Aggregate Query</el-button>
        </span>
</el-dialog>

    </div>
  </template>

  <script>
  // import echarts from 'echarts'	// 引入echarts
  import * as echarts from 'echarts';
  console.log(echarts,'echarts');
      export default {
        data() {
          return {
            search1:"",
            search2:"",
            search3:"",
            dia:{
              search1:"",
              search2:"",
              search3:"",
              search4:"",
              search5:"",
              search6:"",
            },

            dialogVisible:false,
            budgetNum:'',
            chooseNum:"",
            options:[
            {
              value:'DPXPlain',
              label:'DPXPlain'
            },
            {
              value:'db2',
              label:'db2'
            }
          ],
          value:'',
            resultData:[{
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
            rangeRandom(start,end){
              console.log( Math.floor(Math.random()*(end-start+1)+start))
            this.chooseNum= Math.floor(Math.random()*(end-start+1)+start);
          },
        }
      }
    </script>

  <style scoped>
  .containerBox{
    position: relative;
    padding: 50px 50px 50px;
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
    width: 30%;
    height: 200px;
    margin: 0 auto;

    padding-left:50px;
    text-align: left;
  }
    .rightBox >div span{
      display: block;
      margin-top: 5px;
    }
  .rightBox .OR{
    margin: 15px 0;
    display: block;
  }
  .selectBox_{
    margin-bottom: 10px;
  }
  .searchBox,.plusAd {
    width: 100%;
    font-size: 12px;
    position: relative;
  }
  .selectBox1 .el-icon-plus{
      position: absolute;
      font-size: 16px;
      font-weight: 700;
      margin-top: 5px;
      cursor: pointer;
      top: 40px;
  }
  .selectBox1  .plus1{
   right: 30px;
  }
  .selectBox1 .plus2 {
    right: 30px
  }
  .selectBox1 .plus3{
    right: 30px
  }
  .searchBox {
    text-align: left;
  }
  .searchBox .el-select{
    margin:0px  10px;
    width: 136px;
  }
  .el-dialog .el-input{
   width: 200px;
  }
  
  .el-dialog__body >div{
    text-align: left;
     display: flex;
  }
   .el-dialog__body .left{
    width: 10%;
    background: #cccccca6;
    margin-right: 20px;
    text-align: center;
  }
   .el-dialog__body .left div{
      line-height:49px;
      height: 49px;
      cursor: pointer;
   }
   .el-dialog__body .left div:hover{
    background: #ccc;
   }
   .el-dialog__body .right{
    width: 90%;
  }
   .el-dialog__body .right .el-input{
    margin: 0 10px;
   }
   .el-dialog__body .right >div{
    margin-bottom: 10px;
   }
  .el-dialog .AS{
   width: 70px;
  }
   .el-dialog__body span{
    width: 40px;
    height: 50px;
    background: #ccc;
    display: inline-block;
   }
  </style>
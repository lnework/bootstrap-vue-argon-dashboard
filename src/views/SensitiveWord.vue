<template>
  <div>
       <!-- BootstrapVue Done -->
    <base-header class="pb-6 pb-8 pt-5 pt-md-8 bg-gradient-success">

      <el-row :gutter="20" >
        <el-col :span="6">
          <el-input v-model="input" placeholder="输入音频ID号" style="width: 250px"></el-input>

          <el-upload
            drag
            action="https://jsonplaceholder.typicode.com/posts/"
            multiple>
            <i class="el-icon-upload"></i>
            <div class="el-upload__text">将音频拖到此处，或<em>点击上传</em></div>
          </el-upload>

          <el-button type="primary" round  style="width: 250px">音频检测</el-button>
        </el-col>
        <el-col :span="18">
          <el-card shadow="always">
            这是一行文本，特别长.....
          </el-card>
          <el-table
            :data="tableData"
            height="200"
            style="width: 100%">
            <el-table-column
              prop="date"
              label="词类"
              width="180">
            </el-table-column>
            <el-table-column
              prop="name"
              label="风险分"
              width="80">
            </el-table-column>
            <el-table-column
              prop="address"
              label="敏感词">
            </el-table-column>
          </el-table>
        </el-col>

      </el-row>

      <!-- Card stats -->
      <b-row>
        <b-col xl="3" md="4" v-for="stb in sensitiveTableData">
          <stats-card
                      type="gradient-red"
                      :sub-title="stb.name"
                      icon="ni ni-book-bookmark "
                      class="mb-4">

            <template slot="footer">
              <span class="text-danger mr-2">{{ stb.time_rage }}倍</span>
              <span class="text-nowrap">{{stb.description}}</span>
            </template>
          </stats-card>
        </b-col>
      </b-row>
    </base-header>



    <b-container fluid class="mt--7">
      <b-row class="justify-content-center">
        <b-col lg="12">
          <card header-classes="bg-transparent">
            <h3 slot="header" class="mb-0">敏感词</h3>
            <b-row class="icon-examples">
              <b-col lg="2" md="6" v-for="wd in wordData">
                <button type="button" class="btn-icon-clipboard" v-clipboard:copy="'ni ni-active-40'"
                        v-clipboard:success="onCopy"
                        title="1"
                        data-original-title="Copy to clipboard">
                  <div>
                    <span>{{wd.name}}</span>
                    <span class="text-warning mr-2">{{wd.score}}分</span>
                  </div>
                </button>
              </b-col>
            </b-row>

          </card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
  import Vue from 'vue'
  import VueClipboard from 'vue-clipboard2'
  import BaseHeader from '@/components/BaseHeader';
  import projects from "@/views/Tables/projects";

  Vue.use(VueClipboard)
  export default {

    data() {
      return {

        tableData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }]
      ,
        sensitiveTableData: [
          {
            name: '政治敏感',
            time_rage: '3',
            description: '领导人、台湾、香港',

          },
          {
            name: '高危违法',
            time_rage: '2',
            description: '黄赌毒邪教等词语',

          },
          {
            name: '谣言',
            time_rage: '1',
            description: '疫情相关',

          },
          {
            name: '普通违规',
            time_rage: '1',
            description: '辱骂类词汇',

          },


        ],
        wordData:[
          {
            name:'国旗',
            score:'5'
          },
          {
            name:'国歌',
            score:'5'
          },
          {
            name:'天安门',
            score:'5'
          },
          {
            name:'人民英雄纪念碑',
            score:'5'
          },
          {
            name:'军歌',
            score:'5'
          },
          {
            name:'军徽',
            score:'5'
          },
          {
            name:'董存瑞',
            score:'2'
          },
          {
            name:'黄继光',
            score:'2'
          },
          {
            name:'邱少云',
            score:'2'
          },
          {
            name:'王朴',
            score:'2'
          },
          {
            name:'雨来',
            score:'2'
          },
          {
            name:'张自忠',
            score:'2'
          },
          {
            name:'张嘎',
            score:'2'
          },
          {
            name:'内港',
            score:'4'
          },
          {
            name:'内澳',
            score:'4'
          },
          {
            name:'中港',
            score:'4'
          },
          {
            name:'中澳',
            score:'4'
          },
          {
            name:'台独',
            score:'6'
          },
          {
            name:'港独',
            score:'6'
          },
          {
            name:'政变',
            score:'6'
          },
          {
            name:'暴动',
            score:'6'
          },
          {
            name:'枪声',
            score:'6'
          },
          {
            name:'六四',
            score:'6'
          },
          {
            name:'镇压',
            score:'6'
          },
          {
            name:'文革',
            score:'4'
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
          {
            name:'',
            score:''
          },
        ]
      };
    },
    name: 'icons',
    components: {
      BaseHeader
    },
    methods: {
      onCopy() {
        this.$notify({
          type: 'info',
          message: 'Copied to clipboard'
        })
      }
    }
  };
</script>
<style>
.el-row {
  margin-bottom: 20px;
  &:last-child {
     margin-bottom: 0;
   }
}
.el-col {
  border-radius: 4px;
}
.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
.el-upload-dragger{
  width: 250px;
  height: 150px;
  margin-top: 10px;
}
</style>

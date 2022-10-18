<template>
  <div>
       <!-- BootstrapVue Done -->
    <base-header class="pb-1 pb-1 pt-5 pt-md-6 bg-gradient-success">

      <el-row :gutter="20" >
        <el-col :span="4">
        </el-col>
        <el-col :span="8">
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
        <el-col :span="12">

          <el-table
            :data="tableData"
            height="250"
            style="width: 100%">
            <el-table-column
              prop="date"
              label="排序"
              width="80">
            </el-table-column>
            <el-table-column
              prop="name"
              label="种子音频名"
              width="100">
            </el-table-column>
            <el-table-column
              prop="name2"
              label="描述"
              width="200">
            </el-table-column>

            <el-table-column
              prop="name3"
              label="指纹数"
              width="80">
            </el-table-column>

            <el-table-column
              label="相似度"
              width="200"
            >
              <template v-slot="{row}">
                <el-progress :text-inside="true" :stroke-width="24" v-if="row.statusType=='1'" :percentage="100" status="success"></el-progress>
                <el-progress :text-inside="true" :stroke-width="24" v-if="row.statusType!='1'" :percentage="100" status="exception"></el-progress>

              </template>
            </el-table-column>
          </el-table>
        </el-col>

      </el-row>

      <!-- Card stats -->

    </base-header>



    <b-card no-body>
      <b-card-header class="border-0">
        <el-row :gutter="40">
          <el-col :span="6">
            <el-row>
              <el-col :span="6">
                <h4 class="mb-0">音频种子库</h4>
              </el-col>
              <el-col :span="18">
                <b-form-group class="mb-0">
                  <b-input-group class="input-group-alternative input-group-merge">
                    <b-form-input placeholder="输入音频名搜索" type="text"> </b-form-input>

                    <div class="input-group-append">
                      <span class="input-group-text"><i class="fas fa-search"></i></span>
                    </div>
                  </b-input-group>
                </b-form-group>
              </el-col>
            </el-row>

          </el-col>
          <el-col :span="18">
            <el-row :gutter="20" align="right">
              <el-col span="4">
                <h3 class="mb-0">种子上传:</h3>
              </el-col>
              <el-col :span="4">
                <el-input v-model="input" placeholder="事件ID"></el-input>
              </el-col>
              <el-col :span="6">
                <el-input
                  type="textarea"
                  :rows="2"
                  placeholder="描述/备注"
                  v-model="textarea">
                </el-input>
              </el-col>
              <el-col :span="4">
                <el-radio-group v-model="radioTest">
                  <el-radio :label="3">黑库</el-radio>
                  <el-radio :label="6">白库</el-radio>
                </el-radio-group>
                <el-switch
                  v-model="value1"
                  active-text="自动启用"
                  inactive-text="冻结">
                </el-switch>
              </el-col>
              <el-col :span="6">
                <el-upload
                  class="upload-demo"
                  ref="upload"
                  action="https://jsonplaceholder.typicode.com/posts/"
                  :on-preview="handlePreview"
                  :on-remove="handleRemove"
                  :file-list="fileList"
                  :auto-upload="false">
                  <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
                  <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">上传声纹库</el-button>
                </el-upload>
              </el-col>
            </el-row>
          </el-col>
        </el-row>
      </b-card-header>
      <el-row>
        <el-col span="12">
          <el-table class="table-responsive table"
                    header-row-class-name="thead-light"
                    :data="speakerData">
            <el-table-column label="音频名"
                             min-width="30px"
                             prop="name">
              <template v-slot="{row}">
                <b-media no-body class="align-items-center">
                  <b-media-body>
                    <span class="font-weight-600 name mb-0 text-sm">{{row.audio_name}}</span>
                  </b-media-body>
                </b-media>
              </template>
            </el-table-column>
            <el-table-column label="事件"
                             min-width="25px"
                             prop="name">
              <template v-slot="{row}">
                <b-media no-body class="align-items-center">
                  <b-media-body>
                    <span class="font-weight-600 name mb-0 text-sm">{{row.speaker_name}}</span>
                  </b-media-body>
                </b-media>
              </template>
            </el-table-column>
            <el-table-column label="描述"
                             min-width="50px"
                             prop="name">
              <template v-slot="{row}">
                <b-media no-body class="align-items-center">
                  <b-media-body>
                    <span class="font-weight-600 name mb-0 text-sm">{{row.description}}</span>
                  </b-media-body>
                </b-media>
              </template>
            </el-table-column>
            <el-table-column label="操作"
                             min-width="45px"
                             prop="name">
              <template v-slot="{row}">
                <b-media no-body class="align-items-center">
                  <el-button type="info" v-if="row.status=='1'" round>冻结</el-button>
                  <el-button type="success" v-if="row.status=='2'" round>恢复</el-button>
                  <b-media-body>
                    <span class="font-weight-600 name mb-0 text-sm"> <el-button type="danger" round>删除</el-button></span>
                  </b-media-body>
                </b-media>
              </template>
            </el-table-column>



          </el-table>

          <b-card-footer class="py-4 d-flex justify-content-end">
            <base-pagination v-model="currentPage" :per-page="10" :total="50"></base-pagination>
          </b-card-footer>
        </el-col>
        <el-col span="12">
          <el-table class="table-responsive table"
                    header-row-class-name="thead-light"
                    :data="speakerData">
            <el-table-column label="音频名"
                             min-width="30px"
                             prop="name">
              <template v-slot="{row}">
                <b-media no-body class="align-items-center">
                  <b-media-body>
                    <span class="font-weight-600 name mb-0 text-sm">{{row.audio_name}}</span>
                  </b-media-body>
                </b-media>
              </template>
            </el-table-column>
            <el-table-column label="事件"
                             min-width="25px"
                             prop="name">
              <template v-slot="{row}">
                <b-media no-body class="align-items-center">
                  <b-media-body>
                    <span class="font-weight-600 name mb-0 text-sm">{{row.speaker_name}}</span>
                  </b-media-body>
                </b-media>
              </template>
            </el-table-column>
            <el-table-column label="描述"
                             min-width="50px"
                             prop="name">
              <template v-slot="{row}">
                <b-media no-body class="align-items-center">
                  <b-media-body>
                    <span class="font-weight-600 name mb-0 text-sm">{{row.description}}</span>
                  </b-media-body>
                </b-media>
              </template>
            </el-table-column>
            <el-table-column label="操作"
                             min-width="45px"
                             prop="name">
              <template v-slot="{row}">
                <b-media no-body class="align-items-center">
                  <el-button type="info" v-if="row.status=='1'" round>冻结</el-button>
                  <el-button type="success" v-if="row.status=='2'" round>恢复</el-button>
                  <b-media-body>
                    <span class="font-weight-600 name mb-0 text-sm"> <el-button type="danger" round>删除</el-button></span>
                  </b-media-body>
                </b-media>
              </template>
            </el-table-column>



          </el-table>

          <b-card-footer class="py-4 d-flex justify-content-end">
            <base-pagination v-model="currentPage" :per-page="10" :total="50"></base-pagination>
          </b-card-footer>
        </el-col>
      </el-row>

    </b-card>
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

        tableData: [
          {
            date: '1',
            name: '李云龙',
            statusType: '1',
            likeRate: 95
          },
          {
            date: '2',
            name: '李永乐',
            likeRate: 76
          },
          {
            date: '3',
            name: '张三',
            likeRate: 66
          },
          {
            date: '4',
            name: '李四',
            likeRate: 33
          },
          {
            date: '5',
            name: '王五',
            likeRate: 22
          },
        ],
        speakerData:[
          {
            audio_name:'亮剑电视剧',
            speaker_name:'李云龙',
            description:'描述一下',
            status:'1',
          },
          {
            audio_name:'',
            speaker_name:'李云龙',
            description:'',
            status:'2',
          },
          {
            audio_name:'',
            speaker_name:'特朗普',
            description:'',
            status:'',
          },
          {
            audio_name:'',
            speaker_name:'汤家凤',
            description:'',
            status:'',
          },
          {
            audio_name:'',
            speaker_name:'张宇',
            description:'',
            status:'',
          },
          {
            audio_name:'',
            speaker_name:'李永乐',
            description:'',
            status:'',
          },
          {
            audio_name:'',
            speaker_name:'',
            description:'',
            status:'',
          },
        ],
        value1: true,
        radioTest:1,
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
.el-table.table-dark{
  background-color: #172b4d;
  color: #f8f9fe;
}

.el-table.table-dark th,
.el-table.table-dark tr{
  background-color: #172b4d;
}

.el-table.table-dark td,
.el-table.table-dark th.is-leaf{
  border-bottom: none;
}
</style>

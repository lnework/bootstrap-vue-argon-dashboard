<template>
  <div>
       <!-- BootstrapVue Done -->
    <base-header class="pb-2 pb-2 pt-5 pt-md-6 bg-gradient-success">
      <el-row :gutter="40">
        <el-col :span="2">
          &nbsp;
        </el-col>
        <el-col :span="8" style="text-align: right">
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
        <el-col :span="10" style="text-align: left">
          <h3>识别结果</h3>
          <el-table
            :data="tableData"
            height="200"
            style="width: 100%"
          >
            <el-table-column
              prop="date"
              label="排序"
              width="80"
              align="center">
            </el-table-column>
            <el-table-column
              prop="name"
              label="说话人"
              width="100">
            </el-table-column>
            <el-table-column
              label="相似度"
              width="260"
              align="center"
            >
              <template v-slot="{row}">
                <el-progress :text-inside="true" :stroke-width="26" :percentage="row.likeRate"></el-progress>
              </template>
            </el-table-column>
          </el-table>
        </el-col>
        <el-col :span="4">
          &nbsp;
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
                <h3 class="mb-0">声纹库</h3>
              </el-col>
              <el-col :span="18">
                <b-form-group class="mb-0">
                  <b-input-group class="input-group-alternative input-group-merge">
                    <b-form-input placeholder="搜索说话人" type="text"> </b-form-input>

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
                <h3 class="mb-0">声纹上传:</h3>
              </el-col>
              <el-col :span="4">
                <el-input v-model="input" placeholder="说话人ID"></el-input>
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
                <el-switch
                  v-model="value1"
                  active-text="启用"
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

      <el-table class="table-responsive table"
                header-row-class-name="thead-light"
                style="margin-left: 15px"
                :data="speakerData">
        <el-table-column label="音频名"
                         min-width="80px"
                         prop="name">
          <template v-slot="{row}">
            <b-media no-body class="align-items-center">
              <b-media-body>
                <span class="font-weight-600 name mb-0 text-sm">{{row.audio_name}}</span>
              </b-media-body>
            </b-media>
          </template>
        </el-table-column>
        <el-table-column label="说话人名"
                         min-width="80px"
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
                         min-width="180px"
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
                         min-width="110px"
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
            name: '洪七公',
            likeRate: 88
          },
          {
            date: '2',
            name: '李云龙',
            likeRate: 34
          },
          {
            date: '3',
            name: '吴亦凡',
            likeRate: 21
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
            audio_name:'洪七公测试语音.wav',
            speaker_name:'洪七公',
            description:'测试音频，用于检测声纹苦效果',
            status:'1',
          },
          {
            audio_name:'特朗普演讲音频.wav',
            speaker_name:'唐纳德·特朗普',
            description:'美国第45任总统',
            status:'1',
          },
          {
            audio_name:'亮剑电视剧李云龙音频.wav',
            speaker_name:'李云龙',
            description:'亮剑电视剧人物，由于语音特色，常被二次创作恶搞，需监控',
            status:'2',
          },
          {
            audio_name:'郑爽语音.wav',
            speaker_name:'郑爽',
            description:'涉嫌签订阴阳合同、天价片酬、偷税、代孕，为劣迹艺人，严格打击其作品传播',
            status:'1',
          },
          {
            audio_name:'薇娅直播带货高清语音片段.wav',
            speaker_name:'薇娅',
            description:'涉嫌逃税，不能继续直播带货，进行封杀',
            status:'1',
          },
          {
            audio_name:'吴亦凡采访高清语音.wav',
            speaker_name:'吴亦凡',
            description:'涉嫌违法犯罪，为劣迹艺人，其作品全部下架，不予展示',
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

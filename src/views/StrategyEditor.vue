<template>
  <div>
       <!-- BootstrapVue Done -->
    <base-header class="pb-1 pb-1 pt-3 pt-md-6 bg-gradient-success">

    </base-header>



    <b-card no-body >
      <b-card-header class="border-0">
        <el-row gutter="20">
          <el-col span="4">
            <h3 class="mb-0">策略编写器</h3>
          </el-col>
          <el-col span="12">
            <el-select v-model="value2" placeholder="请选择" style="width: 65%">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </el-col>
          <el-col span="8">
            <h3 class="mb-0" style="margin-left: 20px">音频检测</h3>
          </el-col>
        </el-row>
      </b-card-header>
      <div style="margin-left: 50px">
        <el-row gutter="20">
          <el-col span="16">
            <div class="editor-container">
              <yaml-editor v-model="strategyYaml" />
            </div>
          </el-col>
          <el-col span="8">
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
        </el-row>

      </div>

    </b-card>
  </div>
</template>
<script>
  import Vue from 'vue'
  import VueClipboard from 'vue-clipboard2'
  import BaseHeader from '@/components/BaseHeader';
  // import projects from "@/views/Tables/projects";
  //
  import YamlEditor from '@/components/YamlEditor/index.vue';

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
        options: [{
          value: '选项1',
          label: '国庆严格风控风控策略-2022版'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        value2: '选项1',
        value1: true,
        radioTest:1,
        strategyYaml: 'configKey: audio_first_audit\n' +
          'fatalQueueId: 1498619038\n' +
          'qps: 1000\n' +
          'concurrentMode: 1\n' +
          'arbitMode: 1\n' +
          'riskModelConfigs:\n' +
          '- !hust.software.elon.dto.RiskModelConfig\n' +
          '   modelKey: audio_sensitive_word_risk # 敏感词识别\n' +
          '   token: aes-987-uki-623-qwe-072\n' +
          '   qps: 300\n' +
          '   extraJson:\n' +
          '      tableIds: # 需检测的词表Id\n' +
          '         - 1\n' +
          '         - 3\n' +
          '         - 4\n' +
          '      highlight: true # 是否将文本中的敏感词表亮\n' +
          '   finish: false\n' +
          '   shotMinScore: 60.0\n' +
          '   autoDealScore: 90.0\n' +
          '   level: 1 # 仲裁优先级\n' +
          '   tag: audio_sensitive_word_risk_tag\n' +
          '   outputName: audio_sensitive_word_risk\n' +
          '   outputData:\n' +
          '      asr: $.asr\n' +
          '      sensitive_table_match: $.sensitive_tables\n' +
          '      sensitive_word_match: $.sensitive_words\n' +
          '      riskScore: $.score\n' +
          '\n' +
          '- !hust.software.elon.dto.RiskModelConfig\n' +
          '   modelKey: audio_porn_risk # 色情音频识别\n' +
          '   token: aes-987-uki-623-qwe-072\n' +
          '   qps: 300\n' +
          '   extraJson:\n' +
          '      threashold: 0.83\n' +
          '   finish: false\n' +
          '   shotMinScore: 30.0\n' +
          '   autoDealScore: 90.0\n' +
          '   level: 2\n' +
          '   tag: audio_porn_tag\n' +
          '   outputName: audio_porn_risk\n' +
          '   outputData:\n' +
          '      porn_probably: $.porn_probably\n' +
          '      best_porn_probably: $.best_porn_probably\n' +
          '      riskScore: $.score\n' +
          '\n' +
          '- !hust.software.elon.dto.RiskModelConfig\n' +
          '   modelKey: audio_voice_print_risk # 声纹识别\n' +
          '   token: aes-987-uki-623-qwe-072\n' +
          '   qps: 300\n' +
          '   extraJson:\n' +
          '      personIds: # 需检测的说话人\n' +
          '         - 101\n' +
          '         - 132\n' +
          '   finish: false\n' +
          '   shotMinScore: 30.0\n' +
          '   autoDealScore: 90.0\n' +
          '   level: 1 # 仲裁优先级\n' +
          '   tag: audio_sensitive_word_risk_tag\n' +
          '   outputName: audio_sensitive_word_risk\n' +
          '   outputData:\n' +
          '      porn_probably: $.porn_probably # 色情音频的可能性\n' +
          '      riskScore: $.score # 当前阈值所得分数\n' +
          '      best_riskScore: $.best_score # 最佳阈值所得分数\n' +
          '\n' +
          '- !hust.software.elon.dto.RiskModelConfig\n' +
          '   modelKey: audio_similarity_risk # 敏感词识别\n' +
          '   token: aes-987-uki-623-qwe-072\n' +
          '   qps: 300\n' +
          '   extraJson:\n' +
          '      match_count: 65 # 声纹相似点数匹配\n' +
          '   finish: false\n' +
          '   shotMinScore: 30.0\n' +
          '   autoDealScore: 90.0\n' +
          '   level: 1\n' +
          '   tag: audio_similarity_risk_tag\n' +
          '   outputName: audio_similarity_risk\n' +
          '   outputData:\n' +
          '      match_count: $.match_count\n' +
          '      best_match_audio_info: $.best_match_audio_info\n' +
          '      match_audio_info: $.match_audio_info\n' +
          '      riskScore: $.score',
      };
    },
    name: 'icons',
    components: {
      BaseHeader,
      YamlEditor
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
.editor-container{
  position: relative;
  height: 100%;
}
</style>

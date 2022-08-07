<template>
  <a-modal
    :title="title"
    :visible="visible"
    @cancel="handleCancel"
    width="80%"
  >
    <template slot="footer">
      <a-button key="cancel" @click="handleCancel">
        返回
      </a-button>
      <a-button key="ok" type="primary" :loading="confirmLoading" @click="handleOk" v-if="type === 'edit'">
        保存
      </a-button>
    </template>
    <a-row>
      <a-col :span="type === 'view' ? 20 : 24">
        <a-descriptions bordered size="small" :column="4">
          <a-descriptions-item label="巡视反馈重点方面" :span="4">
            {{ rowData.patrolFeedbackKeyAspect }}
          </a-descriptions-item>
          <a-descriptions-item label="巡视反馈重点内容" :span="4">
            {{ rowData.patrolFeedbackKeyContent }}
          </a-descriptions-item>
          <a-descriptions-item label="巡视反馈具体问题" :span="4">
            <div style="word-wrap: break-word; word-break: break-all">
              {{ rowData.patrolFeedbackKeyProblem }}
            </div>
          </a-descriptions-item>
          <a-descriptions-item label="整改计划" :span="4">
            <div style="word-wrap: break-word; word-break: break-all">
              {{ rowData.rectificationPlan }}
            </div>
          </a-descriptions-item>
          <template v-if="type === 'view'">
            <a-descriptions-item label="整改描述" :span="4">
              <div style="word-wrap: break-word; word-break: break-all">
                {{ rowData.rectificationDescription }}
              </div>
            </a-descriptions-item>
          </template>
          <a-descriptions-item label="责任领导" :span="4"> {{ rowData.corporationLeader }} </a-descriptions-item>
          <a-descriptions-item label="分管领导" :span="4"> {{ rowData.branchLeader }} </a-descriptions-item>
          <a-descriptions-item label="整改牵头部门/负责人" :span="4"> {{ rowData.supervisorName }}</a-descriptions-item>
          <a-descriptions-item label="整改时限" :span="4"> {{ rowData.rectificationTime }} </a-descriptions-item>
        </a-descriptions>
      </a-col>
      <a-col :span="type === 'view' ? 4 : 0">
        <div class="circle-parent">
          <span class="progress-circle">
            <a-row>
              <a-progress type="circle" :percent="rowData.lastProgress || 0" />
              <span class="progress-circle-text">上周进度</span>
            </a-row>
          </span>
          <span class="progress-circle">
            <a-row>
              <a-progress type="circle" :percent="rowData.currentProgress || 0" />
              <span class="progress-circle-text">整改进度</span>
            </a-row>
          </span>
        </div>

      </a-col>
      <div v-if="type === 'edit' ">
        <a-divider></a-divider>
        <a-form-model ref="ruleForm" :model="form" :rules="rules" :label-col="labelCol" :wrapper-col="wrapperCol">
          <a-form-model-item ref="name" label="上周进度" >
            <a-row :gutter="48">
              <a-col :span="4">
              </a-col>
              <a-col :span="20">
                <a-progress :percent="+rowData.lastProgress" size="small" />
              </a-col>
            </a-row></a-form-model-item>
          <a-form-model-item ref="name" label="整改进度" prop="currentProgress">
            <a-row :gutter="48">
              <a-col :span="4">
                <a-input-number
                  v-model="form.currentProgress"
                  :min="0"
                  :max="100"
                  :precision="0"
                  :formatter="value => `${value}%`"
                  :parser="value => value.replace('%', '')"
                  @change="manualValidate('progress')"/>
              </a-col>
              <a-col :span="20">
                <a-progress
                  :percent="+form.currentProgress || 0"
                  size="small"/>
              </a-col>

            </a-row>
          </a-form-model-item>

          <a-form-model-item label="整改描述" prop="description">
            <a-input v-model="form.description" placeholder="请输入整改描述" type="textarea" :rows="4" @change="manualValidate('description')"/>
          </a-form-model-item>
        </a-form-model>
      </div>
    </a-row>
  </a-modal>
</template>

<script>
export default {
  name: 'StepForm',
  props: {
    visible: {
      type: Boolean,
      default: false
    },
    type: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: '查看计划'
    },
    rowData: {
      type: Object,
      default: () => ({})
    }
  },
  data () {
    return {
      labelCol: { span: 4 },
      wrapperCol: { span: 14 },
      ModalText: 'Content of the modal',
      confirmLoading: false,
      form: {
        currentProgress: '',
        description: ''
      },
      rules: {
        currentProgress: [
          { required: true, message: '请填写整改进度', trigger: 'blur, change', type: 'number' }
        ],
        description: [
          { required: true, message: '请输入整改描述', trigger: 'blur, change' },
          { max: 255, message: '整改描述长度不超过255个汉字', trigger: 'blur, change' }
        ]
      }
    }
  },
  mounted () {
    this.form.planId = this.rowData.planId
    this.form.currentProgress = +this.rowData?.currentProgress
    this.form.description = this.rowData?.description
  },
  methods: {
    manualValidate (type) {
      this.$refs.ruleForm.validate([type])
    },
    handleOk () {
      this.$refs.ruleForm.validate((valid) => {
        if (valid) {
          this.confirmLoading = true
          console.log(this.form)
          // this.$emit('ok')
        } else {
          this.confirmLoading = false
          this.$message.error('保存失败，请按要求填写数据')
        }
      })
    },
    handleCancel () {
      this.$emit('cancel')
    },
    onSubmit () {}
  }
}
</script>

<style lang="less" scoped>
.steps {
  max-width: 750px;
  margin: 16px auto;
}
.edit-button {
  color: #1961ac;
  &:hover {
    cursor: pointer;
  }
}
.progress-circle {
  margin-top: 30%;
  height: 100%;
  .progress-circle-text{
    margin-left: 24px;
  }
}

.circle-parent {
  display: grid;
  justify-content: center;
  align-items: center;
}
</style>

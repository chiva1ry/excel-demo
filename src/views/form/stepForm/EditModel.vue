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
            巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面
          </a-descriptions-item>
          <a-descriptions-item label="巡视反馈重点内容" :span="4">
            巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容
          </a-descriptions-item>
          <a-descriptions-item label="巡视反馈具体问题" :span="4">
            <div style="word-wrap: break-word; word-break: break-all">
              巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题
              巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题
              巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题
            </div>
          </a-descriptions-item>
          <a-descriptions-item label="整改计划" :span="4">
            <div style="word-wrap: break-word; word-break: break-all">
              整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划
              整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划
              整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划
            </div>
          </a-descriptions-item>
          <template v-if="type === 'view'">
            <a-descriptions-item label="整改描述" :span="4">
              <div style="word-wrap: break-word; word-break: break-all">
                整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述
                整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述
                整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述整改描述
              </div>
            </a-descriptions-item>
          </template>

          <a-descriptions-item label="责任领导" :span="4"> 责任领导 </a-descriptions-item>
          <a-descriptions-item label="分管领导" :span="4"> 责任领导 </a-descriptions-item>
          <a-descriptions-item label="整改牵头部门/负责人" :span="4"> 整改牵头部门/负责人 </a-descriptions-item>
          <a-descriptions-item label="整改时限" :span="4"> 时间格式 </a-descriptions-item>
        </a-descriptions>
      </a-col>
      <a-col :span="type === 'view' ? 4 : 0">
        <span class="progress-circle">
          <a-row>
            <a-progress type="circle" :percent="30" />
            <span class="progress-circle-text">上周进度</span>
          </a-row>
        </span>
        <span class="progress-circle">
          <a-row>
            <a-progress type="circle" :percent="90" />
            <span class="progress-circle-text">本周进度</span>
          </a-row>
        </span>
      </a-col>
      <div v-if="type === 'edit' ">
        <a-divider></a-divider>
        <a-form-model ref="ruleForm" :model="form" :rules="rules" :label-col="labelCol" :wrapper-col="wrapperCol">
          <a-form-model-item ref="name" label="上周进度" >
            <a-row :gutter="48">
              <a-col :span="4">
              </a-col>
              <a-col :span="20">
                <a-progress :percent="+form.formerProgress" size="small" />
              </a-col>
            </a-row></a-form-model-item>
          <a-form-model-item ref="name" label="整改进度" prop="progress">
            <a-row :gutter="48">
              <a-col :span="4">
                <a-input-number v-model="form.progress" :min="0" :max="100" @change="manualValidate"/>
              </a-col>
              <a-col :span="20">
                <a-progress
                  :percent="+form.progress"
                  :formatter="value => `${value}%`"
                  :parser="value => value.replace('%', '')"
                  size="small"/>
              </a-col>

            </a-row>
          </a-form-model-item>

          <a-form-model-item label="整改描述" prop="description">
            <a-input v-model="form.description" type="textarea" :precision="0"/>
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
    }
  },
  data () {
    return {
      labelCol: { span: 4 },
      wrapperCol: { span: 14 },
      ModalText: 'Content of the modal',
      confirmLoading: false,
      form: {
        formerProgress: '80',
        progress: '',
        description: ''
      },
      rules: {
        progress: [
          { required: true, message: '请填写整改进度', trigger: 'blur, change', type: 'number' }
        ],
        description: [
          { required: true, message: '请输入整改描述', trigger: 'blur, change' },
          { max: 500, message: '整改描述最多输入500字', trigger: 'blur, change' }
        ]
      }
    }
  },
  methods: {
    manualValidate () {
      this.$refs.ruleForm.validate(['progress'])
    },
    handleOk () {
      this.$refs.ruleForm.validate((valid) => {
        if (valid) {
          this.confirmLoading = true
          this.$emit('ok')
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
  margin-top: 40%;
  height: 100%;
  display: flex;
  justify-content: space-around;
  .progress-circle-text{
    margin-left: 24px;
  }

}
</style>

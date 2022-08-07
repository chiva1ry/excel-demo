<template>
  <page-header-wrapper>
    <a-card :bordered="false">
      <span>
        查看其他版本：
        <a-select placeholder="请选择版本" style="width: 220px">
          <a-select-option value="jack"> 阿杰 </a-select-option>
          <a-select-option value="lucy"> 阿霖 </a-select-option>
        </a-select>
        <vxe-toolbar ref="xToolbar1" custom></vxe-toolbar>
      </span>
      <!-- <p>
        <template v-for="(column, index) in columns">
          <vxe-checkbox v-model="column.visible" :key="index" @change="$refs.xTable.refreshColumn()">{{
            column.title
          }}</vxe-checkbox>
        </template>
      </p> -->
      <vxe-table
        border
        resizable
        ref="xTable"
        :max-height="height - 525"
        align="center"
        :row-config="{ isCurrent: true, isHover: true }"
        id="xTable1"
        :custom-config="{ storage: true }"
        :data="tableData"
        :loading="loading"
      >
        <vxe-column type="seq" title="序号" width="60"></vxe-column>
        <vxe-column field="patrolFeedbackKeyAspect" title="巡视反馈重点方面" width="160"></vxe-column>
        <vxe-column field="patrolFeedbackKeyContent" title="巡视反馈重点内容"></vxe-column>
        <vxe-column field="patrolFeedbackKeyProblem" title="巡视反馈具体问题"></vxe-column>
        <vxe-column field="rectificationPlan" title="整改计划"></vxe-column>
        <vxe-column field="lastProgress" title="上周进度 %" width="100"></vxe-column>
        <vxe-column field="currentProgress" title="整改进度 %" width="100"></vxe-column>
        <vxe-column field="rectificationDescription" title="整改描述" width="200"></vxe-column>
        <vxe-column field="corporationLeader" title="责任领导" width="100"></vxe-column>
        <vxe-column field="branchLeader" title="分管领导" width="100"></vxe-column>
        <vxe-column field="supervisorName" title="整体牵头部门/负责人" width="200"></vxe-column>
        <vxe-column field="rectificationTime" title="整改时限" width="100"></vxe-column>
        <vxe-column title="操作" width="120">
          <template #default="{ row }">
            <span class="edit-button" @click="editRow(row, 'edit', '编辑计划')">编辑</span>
            <a-divider type="vertical"></a-divider>
            <span class="edit-button" @click="editRow(row, 'view', '查看计划')">查看</span>
          </template>
        </vxe-column>
      </vxe-table>
    </a-card>
    <EditModel
      v-if="visible"
      :visible="visible"
      :type="type"
      :title="title"
      :rowData="rowData"
      @ok="handleOk"
      @cancel="handleCancel"
    ></EditModel>
  </page-header-wrapper>
</template>

<script>
import EditModel from './EditModel.vue'
import json from '../data.js'

export default {
  name: 'StepForm',
  components: {
    EditModel
  },
  data () {
    return {
      height: document.body.scrollHeight,
      title: '查看计划',
      type: 'edit',
      visible: false,
      formData: null,
      rowData: null,
      selectRow: null,
      showEdit: false,
      tableData: null,
      columns: []
    }
  },
  created () {
    this.tableData = json.data.records
  },
  mounted () {
    // 获取所有列配置
    this.$nextTick(() => {
      this.columns = this.$refs.xTable.getColumns()
    })
    console.log(this.columns)
    this.loading = true
    setTimeout(() => {
      // 将指定列设置为隐藏状态
      this.columns.forEach((column) => {
        if (
          [
            'patrolFeedbackKeyAspect',
            'patrolFeedbackKeyContent',
            'patrolFeedbackKeyProblem',
            'rectificationDescription'
          ].includes(column.property)
        ) {
          column.visible = false
        }
      })
      if (this.$refs.xTable) {
        this.$refs.xTable.refreshColumn()
      }
      this.loading = false
    }, 800)
    this.$nextTick(() => {
      // 手动将表格和工具栏进行关联
      this.$refs.xTable.connect(this.$refs.xToolbar1)
    })
  },
  methods: {
    editRow (row, type, title) {
      console.log(row)
      this.visible = true
      this.type = type
      this.title = title
      this.rowData = row
    },
    handleOk () {
      this.visible = false
    },
    handleCancel () {
      this.visible = false
    }
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
</style>

<template>
  <page-header-wrapper>
    <a-card :bordered="false">
      <vxe-toolbar ref="xToolbar1" custom> </vxe-toolbar>
      <vxe-table
        border
        resizable
        ref="xTable"
        :max-height="height - 525"
        align="center"
        :row-config="{ isCurrent: true, isHover: true }"
        :data="tableData"
      >
        <vxe-column type="seq" title="序号" width="60"></vxe-column>
        <vxe-column field="rectificationPlan" title="整改计划"></vxe-column>
        <vxe-column field="lastProgress" title="上周进度 %" width="100"></vxe-column>
        <vxe-column field="currentProgress" title="整改进度 %" width="100"></vxe-column>
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
      tableData: null
    }
  },
  created () {
    this.tableData = json.data.records
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

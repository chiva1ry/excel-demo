<template>
  <!-- hidden PageHeaderWrapper title demo -->
  <page-header-wrapper :title="false" :d="$t('form.basic-form.basic.description')">
    <a-card style="margin-top: 20px">
      查看其他版本：
      <a-select placeholder="请选择版本" style="width: 220px" >
        <a-select-option value="jack">
          阿杰
        </a-select-option>
        <a-select-option value="lucy">
          阿霖
        </a-select-option>
      </a-select>
      <span class="title">
        中铁信科党委关于落实中国中铁党委第三巡视组反馈意见的整改清单
      </span>

      <vxe-table
        ref="xTable"
        border
        resizable
        :max-height="height - 325"
        :scroll-y="{ enabled: false }"
        :span-method="mergeRowMethod"
        :export-config="{}"
        :data="tableData2"
      >
        <vxe-column type="seq" title="序号" width="60"></vxe-column>
        <vxe-column field="patrolFeedbackKeyAspect" title="巡视反馈重点方面" width="160">
        </vxe-column>
        <vxe-column field="patrolFeedbackKeyContent" title="巡视反馈重点内容"></vxe-column>
        <vxe-column field="patrolFeedbackKeyProblem" title="巡视反馈具体问题"></vxe-column>
        <vxe-column field="rectificationPlan" title="整改计划"></vxe-column>
        <vxe-column field="currentProgress" title="整改进度" width="100"></vxe-column>
        <vxe-column field="rectificationDescription" title="整改描述" width="200"></vxe-column>
        <vxe-column field="corporationLeader" title="责任领导" width="100"></vxe-column>
        <vxe-column field="branchLeader" title="分管领导" width="100"></vxe-column>
        <vxe-column field="supervisorName" title="整体牵头部门/负责人" width="200"></vxe-column>
        <vxe-column field="rectificationTime" title="整改时限" width="100"></vxe-column>
      </vxe-table>
    </a-card>
  </page-header-wrapper>
</template>

<script>
import json from '../data.js'
export default {
  data () {
    return {
      height: document.body.scrollHeight,
      tableData: [],
      mergeCells: [],
      // TODO： 责任领导、分管领导、牵头部门/负责人 是否多选
      tableData2: null
    }
  },
  created () {
    this.$nextTick(() => {
      // 手动将表格和工具栏进行关联
      this.$refs.xTable.connect(this.$refs.xToolbar)
    })
    this.tableData = json
    console.log(json)
    this.tableData2 = this.tableData.data.records
  },
  methods: {
    mergeRowMethod ({ row, _rowIndex, column, visibleData }) {
      const fields = ['patrolFeedbackKeyAspect', 'patrolFeedbackKeyContent', 'patrolFeedbackKeyProblem', 'rectificationPlan']
      const cellValue = row[column.property]
      if (cellValue && fields.includes(column.property)) {
        const prevRow = visibleData[_rowIndex - 1]
        let nextRow = visibleData[_rowIndex + 1]
        if (prevRow && prevRow[column.property] === cellValue) {
          return { rowspan: 0, colspan: 0 }
        } else {
          let countRowspan = 1
          while (nextRow && nextRow[column.property] === cellValue) {
            nextRow = visibleData[++countRowspan + _rowIndex]
          }
          if (countRowspan > 1) {
            return { rowspan: countRowspan, colspan: 1 }
          }
        }
      }
    },
    openExportEvent () {
      this.$refs.xTable.openExport()
    }
  }
}
</script>

<style lang="less">
.title {
  font-size: 18px;
  font-weight: bold;
  color: #333;
  margin-bottom: 16px;
  display: flex;
  justify-content: center;
}
</style>

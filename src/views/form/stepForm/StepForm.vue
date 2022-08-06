<template>
  <page-header-wrapper>
    <a-card :bordered="false">
      <vxe-toolbar ref="xToolbar1" custom> </vxe-toolbar>

      <vxe-table
        border
        resizable
        ref="xTable"
        :height="900"
        align="center"
        :row-config="{isCurrent: true, isHover: true}"
        :data="tableData">
        <vxe-column type="seq" title="序号" width="60"></vxe-column>
        <vxe-column field="f" title="整改计划"></vxe-column>
        <vxe-column field="g" title="上周进度 %" width="100"></vxe-column>
        <vxe-column field="g" title="本周进度 %" width="100"></vxe-column>
        <vxe-column field="h" title="责任领导" width="100"></vxe-column>
        <vxe-column field="i" title="分管领导" width="100"></vxe-column>
        <vxe-column field="j" title="整体牵头部门/负责人" width="200"></vxe-column>
        <vxe-column field="k" title="整改时限" width="100"></vxe-column>
        <vxe-column field="k" title="操作" width="120">
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
      @ok="handleOk"
      @cancel="handleCancel"></EditModel>
  </page-header-wrapper>
</template>

<script>
import EditModel from './EditModel.vue'
export default {
  name: 'StepForm',
  components: {
    EditModel
  },
  data () {
    return {
      title: '查看计划',
      type: 'edit',
      visible: false,
      formData: null,
      selectRow: null,
      showEdit: false,
      tableData: [
        {
          id: 10001,
          c: '巡视反馈重点方面-长文本',
          d: '巡视反馈重点内容-长文本',
          e: '巡视反馈具体问题-长文本',
          f: '整改计划-长文本',
          g: '整改进度-数字',
          ga: '整改描述-长文本',
          h: '责任领导-短文本-下拉选字典',
          i: '分管领导-短文本-下拉选字典',
          j: '整体牵头部门/负责人-多选人员',
          k: '整改时限-一个时间'
        },
        {
          id: 10001,
          c: '巡视反馈重点方面-长文本',
          d: '巡视反馈重点内容-长文本',
          e: '巡视反馈具体问题-长文本',
          f: '整改计划-长文本',
          g: '整改进度-数字',
          ga: '整改描述-长文本',
          h: '责任领导-短文本-下拉选字典',
          i: '分管领导-短文本-下拉选字典',
          j: '整体牵头部门/负责人-多选人员',
          k: '整改时限-一个时间'
        }
      ]
    }
  },
  methods: {
    editRow (row, type, title) {
      console.log(row)
      this.visible = true
      this.type = type
      this.title = title
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

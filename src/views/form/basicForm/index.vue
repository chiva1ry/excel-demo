<template>
  <!-- hidden PageHeaderWrapper title demo -->
  <page-header-wrapper :title="false" :d="$t('form.basic-form.basic.description')">
    <a-collapse v-model="activeKey">
      <a-collapse-panel key="1" header="Table1">
        <a-card>
          <vxe-toolbar ref="xToolbar" custom></vxe-toolbar>
          <vxe-table
            border
            resizable
            ref="xTable"
            :height="900"
            align="center"
            :merge-cells="mergeCells"
            :data="tableData"
          >
            <vxe-colgroup field="a" title="大标题">
              <vxe-column field="b" title="序号" width="60"></vxe-column>
              <vxe-column field="c" title="巡视反馈重点方面" width="160"></vxe-column>
              <vxe-column field="d" title="巡视反馈重点内容"></vxe-column>
              <vxe-column field="e" title="巡视反馈具体问题"></vxe-column>
              <vxe-column field="f" title="整改计划"></vxe-column>
              <vxe-column field="g" title="整改进度" width="100"></vxe-column>
              <vxe-column field="h" title="责任领导" width="100"></vxe-column>
              <vxe-column field="i" title="分管领导" width="100"></vxe-column>
              <vxe-column field="j" title="整体牵头部门/负责人" width="200"></vxe-column>
              <vxe-column field="k" title="整改时限" width="100"></vxe-column>
            </vxe-colgroup>
          </vxe-table>
        </a-card>
      </a-collapse-panel>
      <a-collapse-panel key="2" header="Table2">
        <a-card title="card2" style="margin-top: 20px">
          <vxe-table
            border
            resizable
            height="900"
            :scroll-y="{ enabled: false }"
            :span-method="mergeRowMethod"
            :data="tableData2"
          >
            <vxe-colgroup title="大标题">
              <vxe-column field="b" type="seq" title="序号" width="60"></vxe-column>
              <vxe-column field="c" title="巡视反馈重点方面" width="160"></vxe-column>
              <vxe-column field="d" title="巡视反馈重点内容"></vxe-column>
              <vxe-column field="e" title="巡视反馈具体问题"></vxe-column>
              <vxe-column field="f" title="整改计划"></vxe-column>
              <vxe-column field="g" title="整改进度" width="100"></vxe-column>
              <vxe-column field="ga" title="整改描述" width="200"></vxe-column>
              <vxe-column field="h" title="责任领导" width="100"></vxe-column>
              <vxe-column field="i" title="分管领导" width="100"></vxe-column>
              <vxe-column field="j" title="整体牵头部门/负责人" width="200"></vxe-column>
              <vxe-column field="k" title="整改时限" width="100"></vxe-column>
            </vxe-colgroup>
          </vxe-table>
        </a-card>
      </a-collapse-panel>
    </a-collapse>
  </page-header-wrapper>
</template>

<script>
export default {
  data () {
    return {
      activeKey: ['2'],
      tableData: [],
      mergeCells: [],
      // TODO： 责任领导、分管领导、牵头部门/负责人 是否多选
      tableData2: [
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
        },
        { id: 10002, c: 'ccccc1', d: 'content1', e: 'problem1' },
        { id: 10003, c: 'ccccc1', d: 'content2', e: 'problem1' },
        { id: 10004, c: 'ccccc1', d: 'content3', e: 'problem1' },
        { id: 10005, c: 'ccccc2', d: 'content2', e: 'problem2' },
        { id: 10006, c: 'ccccc2', d: 'content2', e: 'problem2' },
        { id: 10007, c: 'ccccc2', d: 'content2', e: 'problem3' },
        { id: 10008, c: 'ccccc2', d: 'content2', e: 'problem3' },
        { id: 10009, c: 'ccccc2', d: 'content1', e: 'problem1' },
        { id: 10010, c: 'ccccc2', d: 'content3', e: 'problem1' },
        { id: 10011, c: 'ccccc3', d: 'content1', e: 'problem4' },
        { id: 10012, c: 'ccccc3', d: 'content2', e: 'problem4' }
      ]
    }
  },
  created () {
    this.$nextTick(() => {
      // 手动将表格和工具栏进行关联
      this.$refs.xTable.connect(this.$refs.xToolbar)
    })
    this.loadList()
  },
  methods: {
    mergeRowMethod ({ row, _rowIndex, column, visibleData }) {
      const fields = ['c', 'd', 'e', 'f']
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
    loadList () {
      const list = []
      for (let index = 1; index < 60; index++) {
        list.push({
          b: index,
          c: '巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面' + index,
          d: '巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容' + index,
          e:
            '巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题' +
            index,
          f:
            '整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划' +
            index,
          g: '整改进度' + index,
          h: '责任领导' + index,
          i: '分管领导' + index,
          j: '整体牵头部门/负责人' + index,
          k: '整改时限' + index
        })
      }
      this.tableData = list
      this.handleMerge()
    },
    handleMerge () {
      // 根据行数据计算合并规则
      const mergeCells = [
        // 巡视反馈重点方面合并
        { row: 0, col: 1, rowspan: 24, colspan: 1 },
        { row: 24, col: 1, rowspan: 15, colspan: 1 },
        { row: 39, col: 1, rowspan: 7, colspan: 1 },
        { row: 46, col: 1, rowspan: 7, colspan: 1 },
        { row: 53, col: 1, rowspan: 4, colspan: 1 },
        { row: 57, col: 1, rowspan: 2, colspan: 1 },

        // 巡视反馈重点内容合并
        { row: 0, col: 2, rowspan: 3, colspan: 1 },
        { row: 3, col: 2, rowspan: 5, colspan: 1 },
        { row: 8, col: 2, rowspan: 6, colspan: 1 },
        { row: 14, col: 2, rowspan: 3, colspan: 1 },
        { row: 17, col: 2, rowspan: 7, colspan: 1 },
        { row: 24, col: 2, rowspan: 4, colspan: 1 },
        { row: 28, col: 2, rowspan: 3, colspan: 1 },
        { row: 31, col: 2, rowspan: 8, colspan: 1 },
        { row: 39, col: 2, rowspan: 7, colspan: 1 },
        { row: 46, col: 2, rowspan: 7, colspan: 1 },
        { row: 53, col: 2, rowspan: 4, colspan: 1 },
        { row: 57, col: 2, rowspan: 2, colspan: 1 },

        // 巡视反馈重点问题合并
        { row: 1, col: 3, rowspan: 2, colspan: 1 },
        { row: 3, col: 3, rowspan: 2, colspan: 1 },
        { row: 33, col: 3, rowspan: 3, colspan: 1 },
        { row: 36, col: 3, rowspan: 3, colspan: 1 },
        { row: 41, col: 3, rowspan: 2, colspan: 1 },
        { row: 24, col: 3, rowspan: 4, colspan: 1 }
      ]
      this.mergeCells = mergeCells
    }
  }
}
</script>

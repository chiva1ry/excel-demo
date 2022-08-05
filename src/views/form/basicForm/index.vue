<template>
  <!-- hidden PageHeaderWrapper title demo -->
  <page-header-wrapper :title="false" :content="$t('form.basic-form.basic.description')">
    <a-card>
      <vxe-toolbar ref="xToolbar" custom></vxe-toolbar>

      <vxe-table
        border
        resizable
        ref="xTable"
        :height="900"
        align="center"
        :merge-cells="mergeCells"
        :data="tableData">
        <vxe-colgroup field="a" title="大标题" >
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

  </page-header-wrapper>
</template>

<script>
export default {
          data () {
            return {
              tableData: [],
              mergeCells: []
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
            loadList () {
              const list = []
              for (let index = 1; index < 60; index++) {
                list.push({
                  b: index,
                  c: '巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面' + index,
                  d: '巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容' + index,
                  e: '巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题' + index,
                  f: '整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划' + index,
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

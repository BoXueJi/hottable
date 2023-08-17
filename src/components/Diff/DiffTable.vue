<template>
  <el-col :span="single ? 24 : 12" class="diff-table">
    <el-header>
      <h4>
        {{ sheetName }}
      </h4>
      <div class="color-description">
        <div>
          修改
          <div class="updateColor"></div>
        </div>
        <div>
          新增
          <div class="insertColor"></div>
        </div>
        <div>
          删除
          <div class="deleteColor"></div>
        </div>
      </div>
    </el-header>
    <el-empty
      v-show="data.length === 0"
      :description="
        sheetName.indexOf('SVN') !== -1 ? '暂无删除或修改' : '暂无新增或修改'
      "
    ></el-empty>
    <div :ref="tableRef" class="hottable" v-show="data.length > 0"></div>
  </el-col>
</template>

<script lang="ts" setup>
import { ref, onMounted, nextTick, getCurrentInstance } from 'vue'
import Handsontable from 'handsontable'
import 'handsontable/dist/handsontable.full.css'
let {
  single,
  sheetName,
  headersCn,
  tableData,
  tableRef,
  diffCells,
  newColumns,
  delColumns,
} = defineProps({
  single: {
    type: Boolean,
    default: false,
  },
  sheetName: {
    type: String,
    required: true,
  },
  headersCn: {
    type: Array,
    required: true,
  },
  tableData: {
    type: Array,
    required: true,
  },
  tableRef: {
    type: String,
    required: true,
  },
  diffCells: {
    type: Object,
    required: true,
  },
  newColumns: {
    type: Array,
    default: null,
  },
  delColumns: {
    type: Array,
    default: null,
  },
})
const emits = defineEmits(['setScrollWrap', 'syncScroll'])

let scrollWrap: HTMLElement
let hotTable: any = null
let data = ref<string[][]>([])

onMounted(() => {
  data.value =
    headersCn.length > 0 ? ([headersCn].concat(tableData) as string[][]) : []
  setHotTable(data.value)
})

const setHotTable = (data: string[][]) => {
  const { proxy } = getCurrentInstance() as any
  const hotWrap = proxy.$refs[tableRef]
  nextTick(() => {
    scrollWrap = hotWrap.getElementsByClassName('wtHolder')[0]
    emits('setScrollWrap', scrollWrap)
  })
  // 获取容器
  // 创建hottable实例
  hotTable = new Handsontable(hotWrap, {
    data, //数据
    colWidths: 120, //单元格宽度
    fixedRowsTop: 1, //固定第一行
    fixedColumnsLeft: 2, //固定左两列
    className: 'htCenter htMiddle', //内容居中
    readOnly: true, //只读
    cells: setCells, //渲染单元格
    afterScrollHorizontally,
    licenseKey: 'non-commercial-and-evaluation',
  })
}

const setCells = (row: number) => {
  if (newColumns || delColumns) {
    return {
      renderer: cellsColorRenderer,
    }
  } else if (
    diffCells.hasOwnProperty((data.value[row] as Array<string>).join('-'))
  ) {
    return {
      renderer: cellsColorRenderer,
    }
  }
  return {}
}

const cellsColorRenderer = (
  instance: any,
  td: HTMLTableCellElement,
  row: number,
  col: number,
  prop: any,
  value: any,
  cellProperties: any
) => {
  Handsontable.renderers.TextRenderer.apply(this, [
    instance,
    td,
    row,
    col,
    prop,
    value,
    cellProperties,
  ])
  let key = (data.value[row] as Array<string>).join('-')
  // 新增列的内容染色
  if (newColumns && newColumns.includes(col)) {
    td.style.backgroundColor = '#67C23A'
    td.style.color = '#fff'
  }
  // 删除列的内容染色
  if (delColumns && delColumns.includes(col)) {
    td.style.backgroundColor = '#F56C6C'
    td.style.color = '#fff'
  }
  // 行染色
  if (diffCells.hasOwnProperty(key)) {
    if (diffCells[key] == 'add') {
      td.style.backgroundColor = '#67C23A'
      td.style.color = '#fff'
    } else if (diffCells[key] == 'del') {
      td.style.backgroundColor = '#f56c6c'
      td.style.color = '#fff'
    } else if (col == 0 || diffCells[key].includes(col)) {
      td.style.backgroundColor = '#E6A23C'
      td.style.color = '#fff'
    }
  }
  return td
}

const afterScrollHorizontally = () => {
  if (single) {
    return
  }
  emits('syncScroll', {
    direction: 'horizontally',
    tableRef: tableRef,
    scrollTo: scrollWrap.scrollLeft,
  })
}
</script>
<style lang="scss">
.diff-table {
  display: flex !important;
  flex-direction: column;
  height: 74vh;
  width: 100%;
  .el-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: auto;
    margin-bottom: 0.3571rem;
    .color-description {
      font-size: 14px;
      color: #606266;
      div {
        display: inline-block;
        vertical-align: middle;
        margin-right: 0.2143rem;
        .updateColor {
          background-color: #e6a23c;
        }
        .insertColor {
          background-color: #67c23a;
        }
        .deleteColor {
          background-color: #f56c6c;
        }
        div {
          width: 16px;
          height: 16px;
        }
      }
    }
  }
  .hottable {
    width: 100%;
    flex: 1;
    border-radius: 6px;
    overflow: auto;
    border: 1px solid #cccccc;
    border-bottom: 0px solid #cccccc;
    .handsontable {
      height: 100%;
      .ht_master tr th {
        visibility: visible;
      }
    }
    .wtHolder {
      height: 100% !important;
      box-sizing: border-box;
    }
    .handsontable {
      tbody {
        th {
          border-left: 0px solid #ccc;
        }
        td {
          word-break: break-all;
        }
        & tr:first-of-type {
          td {
            border-top: 0px transparent;
            background-color: #f0f0f0;
            color: black;
            white-space: nowrap;
          }
        }
        & > tr > td {
          & :first-of-type {
            border-left: 0px solid #ccc;
          }
          &:first-of-type {
            border-left: 0px solid #ccc;
          }
          &:nth-of-type(2) {
            border-right: 1px solid #ccc;
          }
        }
      }
    }
  }
  & > .hottable {
    overflow: hidden;
  }
}
</style>

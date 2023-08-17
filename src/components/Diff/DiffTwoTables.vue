<template>
  <!-- 之前版本数据数据 -->
  <DiffTable
    :tableData="props.earlyData"
    :headersCn="props.earlyHeadersCn"
    :sheetName="`${props.sheetName}---SVN`"
    :diffCells="props.earlyDiffCells"
    :delColumns="props.delColumns"
    tableRef="earlyTable"
    @setScrollWrap="setEarlyTable"
    @syncScroll="syncScroll"
  ></DiffTable>
  <!-- 当前版本数据数据 -->
  <DiffTable
    :tableData="props.currentData"
    :headersCn="props.currentHeadersCn"
    :sheetName="`${props.sheetName}---草稿箱 `"
    :diffCells="props.currentDiffCells"
    :newColumns="props.newColumns"
    tableRef="currentTable"
    @setScrollWrap="setCurrentTable"
    @syncScroll="syncScroll"
  ></DiffTable>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import DiffTable from './DiffTable.vue'
const props = defineProps({
  earlyData: {
    type: Array,
    required: true,
  },
  currentData: {
    type: Array,
    required: true,
  },
  earlyHeadersCn: {
    type: Array,
    required: true,
  },
  currentHeadersCn: {
    type: Array,
    required: true,
  },
  sheetName: {
    type: String,
    required: true,
  },
  earlyDiffCells: {
    type: Object,
    required: true,
  },
  currentDiffCells: {
    type: Object,
    required: true,
  },
  newColumns: {
    type: Array,
    deault: null,
  },
  delColumns: {
    type: Array,
    deault: null,
  },
})

let earlyTable = ref<HTMLElement>()
let currentTable = ref<HTMLElement>()

const setEarlyTable = (el: HTMLElement) => {
  earlyTable.value = el
}

const setCurrentTable = (el: HTMLElement) => {
  currentTable.value = el
}

const syncScroll = (data: {
  direction: string
  tableRef: string
  scrollTo: number
}) => {
  if (data.direction === 'horizontally') {
    if (data.tableRef === 'earlyTable') {
      currentTable.value!.scrollLeft = data.scrollTo
    } else {
      earlyTable.value!.scrollLeft = data.scrollTo
    }
  }
}
</script>
<style lang="scss" scoped></style>

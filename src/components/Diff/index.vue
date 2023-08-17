<template>
  <DialogTemplate
    :visible="visible"
    @close="closeDialog"
    width="95%"
    top="60px"
  >
    <template v-slot:header>
      <h4>
        {{ '查看文件差异' }}
      </h4>
    </template>
    <template v-slot:body>
      <el-tabs
        type="card"
        v-model="activeTab"
        @tab-click="changeTab"
        class="diff_body"
      >
        <el-tab-pane
          v-for="(item, index) in tabs"
          :key="index"
          :label="item"
          :name="item"
        >
          <el-row :gutter="5" v-if="item === activeTab && !loading">
            <DiffTwoTables
              :early-data="diffData['sheet1'].earlyData"
              :current-data="diffData['sheet1'].currentData"
              :early-headers-cn="diffData['sheet1'].earlyHeadersCn"
              :current-headers-cn="diffData['sheet1'].currentHeadersCn"
              :sheet-name="diffData['sheet1'].sheetName"
              :early-diff-cells="diffData['sheet1'].earlyDiffCells"
              :current-diff-cells="diffData['sheet1'].currentDiffCells"
              :newColumns="diffData['sheet1'].newColumns"
              :delColumns="diffData['sheet1'].delColumns"
            ></DiffTwoTables>
          </el-row>
        </el-tab-pane>
      </el-tabs>
      <div class="toggle-loading" v-show="loading">
        <el-icon class="is-loading"><Loading /></el-icon>
        <span>处理数据中，请稍等...</span>
      </div>
    </template>
  </DialogTemplate>
</template>

<script lang="ts" setup>
import { ref, onMounted, computed } from 'vue'
import DialogTemplate from '../DialogTemplate/index.vue'
import DiffTwoTables from './DiffTwoTables.vue'
interface Arbitrary {
  [key: string]: any
}
const props = defineProps({
  visible: {
    type: Boolean,
    required: true,
  },
})
const emits = defineEmits(['close'])
let visible = computed({
  get() {
    return props.visible
  },
  set() {
    emits('close')
  },
})
let tabs = ref<Array<string>>([])
// 当前tab
let activeTab = ref('')
let diffData = ref<Arbitrary>({})
let loading = ref(false)

onMounted(() => {
  tabs.value = ['sheet1']
  activeTab.value = tabs.value[0]
  diffData.value['sheet1'] = {
    sheetName: 'sheet1',
    earlyData: [
      ['2', '1,1', '1,3'],
      ['3', '2,1', '2,3'],
      ['4', '3,1', '3,3'],
      ['5', '4,1', '4,3'],
      ['6', '5,1', '5,3'],
      ['7', '6,1', '6,3'],
      ['8', '7,1', '7,3'],
      ['9', '8,1', '8,3'],
      ['10', '9,1', '9,3'],
      ['11', '10,1', '10,3'],
      ['12', '11,1', '11,3'],
      ['13', '12,1', '12,3'],
      ['14', '13,1', '13,3'],
      ['15', '14,1', '14,3'],
      ['16', '15,1', '15,3'],
      ['17', '16,1', '16,3'],
      ['18', '17,1', '17,3'],
      ['19', '18,1', '18,3'],
      ['20', '19,1', '19,3'],
      ['21', '20,1', '20,3'],
      ['22', '21,1', '21,3'],
      ['23', '22,1', '22,3'],
      ['24', '23,1', '23,3'],
      ['25', '24,1', '24,3'],
      ['26', '25,1', '25,3'],
      ['27', '26,1', '26,3'],
      ['28', '27,1', '27,3'],
      ['29', '28,1', '28,3'],
      ['30', '29,1', '29,3'],
      ['31', '30,1', '30,3'],
      ['32', '31,1', '31,3'],
      ['33', '32,1', '32,3'],
      ['34', '33,1', '33,3'],
      ['35', '34,1', '34,3'],
      ['36', '35,1', '35,3'],
      ['37', '36,1', '36,3'],
      ['38', '37,1', '37,3'],
      ['39', '38,1', '38,3'],
      ['40', '39,1', '39,3'],
      ['41', '40,1', '40,3'],
      ['42', '41,1', '41,3'],
      ['43', '42,1', '42,3'],
      ['44', '43,1', '43,3'],
      ['45', '44,1', '44,3'],
      ['46', '45,1', '45,3'],
      ['47', '46,1', '46,3'],
      ['48', '47,1', '47,3'],
      ['49', '48,1', '48,3'],
      ['50', '49,1', '49,3'],
      ['51', '50,1', '50,3'],
    ],
    currentData: [
      ['2', '1,1', '1,2', '1,3'],
      ['3', '2,1', '2,2', '2,3'],
      ['4', '3,1', '3,2', '3,3'],
      ['5', '4,1', '4,2', '4,3'],
      ['6', '5,1', '5,2', '5,3'],
      ['7', '6,1', '6,2', '6,3'],
      ['8', '7,1', '7,2', '7,3'],
      ['9', '8,1', '8,2', '8,3'],
      ['10', '9,1', '9,2', '9,3'],
      ['11', '10,1', '10,2', '10,3'],
      ['12', '11,1', '11,2', '11,3'],
      ['13', '12,1', '12,2', '12,3'],
      ['14', '13,1', '13,2', '13,3'],
      ['15', '14,1', '14,2', '14,3'],
      ['16', '15,1', '15,2', '15,3'],
      ['17', '16,1', '16,2', '16,3'],
      ['18', '17,1', '17,2', '17,3'],
      ['19', '18,1', '18,2', '18,3'],
      ['20', '19,1', '19,2', '19,3'],
      ['21', '20,1', '20,2', '20,3'],
      ['22', '21,1', '21,2', '21,3'],
      ['23', '22,1', '22,2', '22,3'],
      ['24', '23,1', '23,2', '23,3'],
      ['25', '24,1', '24,2', '24,3'],
      ['26', '25,1', '25,2', '25,3'],
      ['27', '26,1', '26,2', '26,3'],
      ['28', '27,1', '27,2', '27,3'],
      ['29', '28,1', '28,2', '28,3'],
      ['30', '29,1', '29,2', '29,3'],
      ['31', '30,1', '30,2', '30,3'],
      ['32', '31,1', '31,2', '31,3'],
      ['33', '32,1', '32,2', '32,3'],
      ['34', '33,1', '33,2', '33,3'],
      ['35', '34,1', '34,2', '34,3'],
      ['36', '35,1', '35,2', '35,3'],
      ['37', '36,1', '36,2', '36,3'],
      ['38', '37,1', '37,2', '37,3'],
      ['39', '38,1', '38,2', '38,3'],
      ['40', '39,1', '39,2', '39,3'],
      ['41', '40,1', '40,2', '40,3'],
      ['42', '41,1', '41,2', '41,3'],
      ['43', '42,1', '42,2', '42,3'],
      ['44', '43,1', '43,2', '43,3'],
      ['45', '44,1', '44,2', '44,3'],
      ['46', '45,1', '45,2', '45,3'],
      ['47', '46,1', '46,2', '46,3'],
      ['48', '47,1', '47,2', '47,3'],
      ['49', '48,1', '48,2', '48,3'],
      ['50', '49,1', '49,2', '49,3'],
      ['51', '50,1', '50,2', '50,3'],
    ],
    earlyHeadersCn: ['', 'A1', 'A3', 'A4'],
    currentHeadersCn: ['', 'A1', 'A2', 'A3'],
    earlyDiffCells: {},
    currentDiffCells: {},
    newColumns: [2],
    delColumns: [3],
  }
})
const changeTab = () => {}
const closeDialog = () => {
  visible.value = false
}
</script>
<style lang="scss" scoped>
.toggle-loading {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%);
  color: #409eff;
  display: flex;
  align-items: center;
  z-index: 1000;
  span {
    margin-left: 5px;
  }
}
.diff_body {
  height: calc(74vh + 45px);
}
.el-tabs {
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow: auto;
  background-color: #f5f7fa;
  :deep(.el-tabs__header) {
    display: flex;
    margin-bottom: 0px;
  }

  :deep(.el-tabs__nav) {
    background-color: #eaf2fa;
    border: none !important;
    border-bottom: 1px solid #94b7dc !important;
  }
  :deep(.el-tabs__item) {
    color: #909399;
    border: 1px solid #94b7dc !important;
  }
  :deep(.el-tabs__item):nth-of-type(n + 2) {
    border-left: none !important;
  }
  :deep(.el-tabs__item.is-active) {
    border: 1px solid #409eff !important;
    background-color: #409eff;
    color: #fff;
  }
  :deep(.el-tabs__content) {
    padding: 5px 10px 10px;
  }
}
.el-empty {
  height: calc(74vh + 40px);
}
</style>
<style>
.el-tabs__nav {
  border: none;
}
</style>

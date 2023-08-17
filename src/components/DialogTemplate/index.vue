<template>
  <el-dialog
    v-model="visible"
    :width="width"
    :close-on-click-modal="false"
    :top="top"
    append-to-body
    class="dialog-template"
    destroy-on-close
  >
    <template #header>
      <slot name="header"></slot>
    </template>
    <slot name="body"></slot>
    <template #footer>
      <slot name="footer"></slot>
    </template>
  </el-dialog>
</template>

<script lang="ts" setup>
import { computed } from 'vue'
const props = defineProps({
  visible: {
    type: Boolean,
    required: true,
  },
  width: {
    type: String,
    default: '60%',
  },
  top: {
    type: String,
    default: '5%',
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
</script>
<style lang="scss">
.dialog-template {
  .el-dialog__body {
    margin-top: 10px;
    padding: 0 15px;
  }
}
</style>

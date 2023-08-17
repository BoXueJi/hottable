<script setup lang="ts">
import Diff from './components/Diff/index.vue'
import { ref } from 'vue'
let visible = ref(false)
</script>

<template>
  <el-button @click="visible = true" type="primary">open</el-button>
  <Diff v-if="visible" :visible="visible" @close="visible = false"></Diff>
</template>

<style scoped></style>

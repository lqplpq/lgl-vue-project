<script setup>
import { defineEmits, defineProps } from 'vue'

const props = defineProps({
    visible: Boolean,
})
const emit = defineEmits(['update:visible'])

const close = () => emit('update:visible', false)
</script>

<template>
    <el-dialog :model-value="props.visible" width="520px" @close="close" custom-class="my-dialog">
        <template #title>
            <slot name="title">弹窗标题</slot>
        </template>

        <div class="dialog-body">
            <slot name="content">这里是弹窗的主要内容。</slot>
        </div>

        <template #footer>
            <el-button type="primary" @click="close">关闭</el-button>
        </template>
    </el-dialog>
</template>

<style>
.dialog-body {
    padding: 0.75rem 0;
    color: #606266;
    line-height: 1.7;
}

.my-dialog {
    --el-dialog-margin-top: 0 !important;
    margin: 0 auto !important;
}

.my-global-dialog {
    --el-dialog-margin-top: 0 !important;
    margin: 0 !important;
    position: fixed !important;
    top: 50% !important;
    left: 50% !important;
    transform: translate(-50%, -50%) !important;
}

.el-dialog {
    /* --el-dialog-margin-top: 0 !important; */
    /* margin: 0 !important;
    position: fixed !important;
    top: 50% !important;
    left: 50% !important;
    transform: translate(-50%, -50%) !important; */
}

/* :deep(.el-dialog) {
    --el-dialog-margin-top: 0 !important;
   
    position: fixed !important;
    top: 50% !important;
    left: 50% !important;
    transform: translate(-50%, -50%) !important;
    margin: 0 !important;
}

:deep(.full-center-dialog .el-dialog) {
    margin: 0 !important;
    
    position: fixed !important;
    top: 50% !important;
    left: 50% !important;
    transform: translate(-50%, -50%) !important;
} */
</style>

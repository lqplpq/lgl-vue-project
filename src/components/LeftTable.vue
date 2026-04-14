<script setup>
import { defineEmits, defineProps } from 'vue'

const { rows, selectedId } = defineProps({
    rows: { type: Array, default: () => [] },
    selectedId: Number,
})
const emit = defineEmits(['select-row', 'toggle-check', 'open-dialog'])

const selectRow = (row) => emit('select-row', row.id)
const toggleCheck = (row, checked) => emit('toggle-check', { id: row.id, checked })
const openDialog = (row) => emit('open-dialog', row)
</script>

<template>
    <el-card class="panel-card">
        <div class="panel-heading">
            <div>
                <h2>左侧数据</h2>
                <p>点击行切换右侧评分明细，勾选复选框用于批量操作，最后一列可打开详情弹窗。</p>
            </div>
        </div>

        <el-table :data="rows" stripe highlight-current-row
            :row-class-name="({ row }) => (row.id === selectedId ? 'is-selected-row' : '')" @row-click="selectRow"
            size="medium" border>
            <el-table-column label="选择" width="100">
                <template #default="{ row }">
                    <el-checkbox v-model="row.checked" @change="(checked) => toggleCheck(row, checked)" @click.stop />
                </template>
            </el-table-column>
            <el-table-column prop="id" label="编号" width="90" />
            <el-table-column prop="name" label="名称" />
            <el-table-column prop="status" label="状态" width="110" />
            <el-table-column prop="owner" label="负责人" width="110" />
            <el-table-column label="质量" width="140">
                <template #default="{ row }">
                    <div class="quality-selector" @click.stop>
                        <el-radio-group v-model="row.quality" size="small">
                            <el-radio label="合格">合格</el-radio>
                            <el-radio label="不合格">不合格</el-radio>
                        </el-radio-group>
                    </div>
                </template>
            </el-table-column>
            <el-table-column prop="total" label="总分" width="100" />
            <el-table-column label="操作" width="120">
                <template #default="{ row }">
                    <el-button size="small" type="primary" @click.stop="openDialog(row)">详情</el-button>
                </template>
            </el-table-column>
        </el-table>
    </el-card>
</template>

<style scoped>
.panel-card {
    padding: 0.5rem;
}

.panel-heading {
    margin-bottom: 1rem;
}

h2 {
    margin: 0;
    font-size: 1.15rem;
}

.panel-heading p {
    margin: 0.4rem 0 0;
    color: #606266;
    font-size: 0.92rem;
}

.is-selected-row {
    background: #f0f9eb !important;
}
</style>

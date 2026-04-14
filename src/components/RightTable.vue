<script setup>
import { computed, defineEmits, defineProps } from 'vue'

const props = defineProps({
    rows: { type: Array, default: () => [] },
    currentName: String,
})
const emit = defineEmits(['update:rows', 'save'])

const canSave = computed(() => {
    return props.rows.length === 10 && props.rows.every((item) => {
        return typeof item.value === 'number' && item.value >= 1 && item.value <= 10
    })
})

const onInput = (row, value) => {
    emit(
        'update:rows',
        props.rows.map((item) =>
            item.id === row.id ? { ...item, value: value === null ? null : Number(value) } : item
        )
    )
}

const onSave = () => {
    if (canSave.value) {
        emit('save')
    }
}
</script>

<template>
    <el-card class="panel-card">
        <div class="panel-heading">
            <div>
                <h2>右侧评分明细</h2>
                <p>当前选中：{{ currentName || '请先选择左侧行' }}</p>
            </div>
            <div>
                <el-tag type="success" v-if="canSave">可保存</el-tag>
                <el-tag type="warning" v-else>请完成 10 行 1-10 分输入</el-tag>
            </div>
        </div>

        <el-table :data="rows" stripe size="medium" border>
            <el-table-column prop="id" label="序号" width="90" />
            <el-table-column prop="label" label="项目" />
            <el-table-column label="得分" width="170">
                <template #default="{ row }">
                    <el-input-number :model-value="row.value" @update:model-value="(value) => onInput(row, value)"
                        :min="1" :max="10" controls-position="right" size="small" style="width: 100%;" />
                </template>
            </el-table-column>
        </el-table>

        <div class="actions">
            <el-button type="primary" :disabled="!canSave" @click="onSave">保存</el-button>
        </div>
    </el-card>
</template>

<style scoped>
.panel-card {
    padding: 0.5rem;
}

.panel-heading {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
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

.actions {
    margin-top: 1rem;
    display: flex;
    justify-content: flex-end;
}
</style>

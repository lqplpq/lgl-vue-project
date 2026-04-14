<script setup>
import { computed, reactive, ref, watch } from 'vue'
import LeftTable from '../components/LeftTable.vue'
import RightTable from '../components/RightTable.vue'
import ModalDialog from '../components/ModalDialog.vue'

const leftRows = reactive([
    { id: 1, name: '项目 A', status: '待处理', owner: '张三', total: 0, checked: false, quality: '合格' },
    { id: 2, name: '项目 B', status: '进行中', owner: '李四', total: 0, checked: false, quality: '合格' },
    { id: 3, name: '项目 C', status: '已完成', owner: '王五', total: 0, checked: false, quality: '合格' },
])

const selectedId = ref(leftRows[0].id)
const showDialog = ref(false)
const dialogRow = ref(null)

const rightDataMap = reactive({
    1: generateRightRows('A'),
    2: generateRightRows('B'),
    3: generateRightRows('C'),
})

const currentRows = ref(rightDataMap[selectedId.value].map((item) => ({ ...item })))

function generateRightRows(tag) {
    return Array.from({ length: 10 }, (_, index) => ({
        id: index + 1,
        label: `${tag} 明细 ${index + 1}`,
        value: null,
    }))
}

watch(selectedId, (id) => {
    currentRows.value = rightDataMap[id].map((item) => ({ ...item }))
})

const currentName = computed(() => leftRows.find((item) => item.id === selectedId.value)?.name)

const selectRow = (id) => {
    selectedId.value = id
}

const toggleCheck = ({ id, checked }) => {
    const row = leftRows.find((item) => item.id === id)
    if (row) {
        row.checked = checked
    }
}

const openDialog = (row) => {
    dialogRow.value = row
    showDialog.value = true
}

const updateRows = (rows) => {
    currentRows.value = rows
}

const saveRight = () => {
    const isValid = currentRows.value.every((item) => {
        return typeof item.value === 'number' && item.value >= 1 && item.value <= 10
    })

    if (!isValid) {
        return
    }

    const total = currentRows.value.reduce((sum, item) => sum + Number(item.value), 0)
    const row = leftRows.find((item) => item.id === selectedId.value)
    if (row) {
        row.total = total
    }

    rightDataMap[selectedId.value] = currentRows.value.map((item) => ({ ...item }))
}
</script>

<template>
    <main class="page-shell">
        <section class="page-head">
            <el-row align="middle" justify="space-between" class="page-head-row">
                <el-col :span="24">
                    <p class="badge">评分联动页面</p>
                    <h1>左侧层级选择 + 右侧评分输入</h1>
                    <p>左侧点击行切换右侧 10 行评分输入，全部填写后可保存，保存后会将总分同步到左侧表格。</p>
                </el-col>
            </el-row>
        </section>

        <section class="page-body">
            <LeftTable :rows="leftRows" :selectedId="selectedId" @select-row="selectRow" @toggle-check="toggleCheck"
                @open-dialog="openDialog" />
            <RightTable :rows="currentRows" :currentName="currentName" @update:rows="updateRows" @save="saveRight" />
        </section>

        <ModalDialog :visible="showDialog" @update:visible="showDialog = $event">
            <template #title>行详情</template>
            <template #content>
                <p><strong>名称：</strong>{{ dialogRow?.name }}</p>
                <p><strong>状态：</strong>{{ dialogRow?.status }}</p>
                <p><strong>负责人：</strong>{{ dialogRow?.owner }}</p>
                <p><strong>当前总分：</strong>{{ dialogRow?.total }}</p>
                <p>这是左侧表格单行的明细弹窗，点击“详情”按钮可打开。</p>
            </template>
        </ModalDialog>
    </main>
</template>

<style scoped>
.page-shell {
    padding: 2rem 1rem;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
    background: #f3f4f6;
}

.page-head {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    max-width: 1180px;
    margin: 0 auto;
}

.page-head-row {
    width: 100%;
}

.badge {
    display: inline-flex;
    padding: 0.35rem 0.75rem;
    border-radius: 999px;
    background: #e0e7ff;
    color: #3730a3;
    font-size: 0.8rem;
    font-weight: 700;
}

h1 {
    margin: 0.75rem 0 0;
    font-size: clamp(2rem, 2.5vw, 2.75rem);
    line-height: 1.1;
}

.page-body {
    display: grid;
    grid-template-columns: 1.1fr 0.9fr;
    gap: 1.5rem;
    max-width: 1180px;
    width: 100%;
    margin: 0 auto;
}

@media (max-width: 900px) {
    .page-body {
        grid-template-columns: 1fr;
    }
}
</style>

<script lang="ts" setup>
const props = defineProps({
    productData: Object, 
})
const deleteRow = (index: number) => {
    productData.value.splice(index, 1)
}
</script>

<template>
    <el-table :data="productData" style="width: 100%" max-height="250">
        <el-table-column fixed prop="date" label="产品编号" width="150" />
        <el-table-column prop="name" label="产品名称" width="120" />
        <el-table-column prop="name" label="相关信息" width="120" />
        <el-table-column fixed="right" label="操作" width="120">
            <template #default="scope">
                <el-button link type="primary" size="small" @click.prevent="deleteRow(scope.$index)">修改产品</el-button>
                <el-button link type="primary" size="small" @click.prevent="deleteRow(scope.$index)">删除产品</el-button>
            </template>
        </el-table-column>
    </el-table>
</template>

<style scoped>
</style>
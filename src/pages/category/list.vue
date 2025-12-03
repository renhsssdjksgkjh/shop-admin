<template>
    <el-card shadow="never" class="border-0">

        <ListHeader @refresh="getData" @create="handleCreate"></ListHeader>

        <el-tree :data="tableData" :props="{ label: 'name', children: 'child' }" v-loading="loading" node-key="id">
            <template #default="{ node, data }">
                <div class="custom-tree-node">
                    <span>{{ data.name }}</span>
                    <div class="ml-auto">
                        <el-switch :modelValue="data.status" :active-value="1" :inactive-value="0"
                            @change="handleStatusChange($event, data)" />
                        <el-button type="primary" text size="small" @click.stop="handleEdit(data)">修改</el-button>
                        <el-button type="primary" text size="small" @click.stop="addChild(data.id)">增加</el-button>
                        <el-popconfirm width="220" title="是否确定要删除该菜单？" confirm-button-text="确认" cancel-button-text="取消"
                            @confirm="handleDelete(data.id)">
                            <template #reference>
                                <el-button type="primary" text size="small">删除</el-button>
                            </template>
                        </el-popconfirm>
                    </div>
                </div>
            </template>
        </el-tree>
        <FormDrawer ref="formDrawerRef" :title="drawerTitle" @submit="handleSubmit">
            <el-form :model="form" ref="formRef" :rules="rules" label-width="80px" :inline="false">
                <el-form-item label="分类名称" prop="name">
                    <el-input v-model="form.name" placeholder="名称"></el-input>
                </el-form-item>
            </el-form>
        </FormDrawer>
    </el-card>
</template>

<script setup>
import ListHeader from '~/components/ListHeader.vue'
import { useInitTable, useInitForm } from '~/composables/useCommon.js'
import { getCategoryList, createCategory, updateCategory, updateCategoryStatus, deleteCategory } from '~/api/category.js'
import FormDrawer from '~/components/FormDrawer.vue'

const {
    getData,
    loading,
    tableData,
    handleDelete,
    handleStatusChange
} = useInitTable({
    getList: getCategoryList,
    onGetListSuccess: (res) => {
        tableData.value = res
    },
    delete: deleteCategory,
    updateStatus: updateCategoryStatus
})

const {
    formDrawerRef,
    formRef,
    form,
    rules,
    drawerTitle,
    handleSubmit,
    resetForm,
    handleCreate,
    handleEdit
} = useInitForm({
    rules: {},
    form: {
        name: ''
    },
    getData,
    update: updateCategory,
    create: createCategory
})

const addChild = (id) => {
    handleCreate()
    form.rule_id = id
    form.status = 1
}

</script>

<style>
.custom-tree-node {
    flex: 1;
    display: flex;
    align-items: center;
    font-size: 14px;
    padding-right: 8px;
}

.el-tree-node__content {
    padding: 20px 0;
}
</style>
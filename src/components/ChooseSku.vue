<template>
    <el-dialog title="规格选择" v-model="dialogVisible" width="80%" top="5vh">
        <el-container style="height:65vh">
            <el-aside width="220px">
                <div class="top">
                    <aside-list @click="handleChangeActiveId(item.id)" @edit="handleEdit(item)"
                        @delete="handledelete(item.id)" v-for="(item, index) in list" :key="index"
                        :active="activeId == item.id">{{ item.name
                        }}</aside-list>
                </div>
                <div class="bottom">
                    <el-pagination background layout="prev,next" :total="total" :current-page="currentPage"
                        :page-size="limit" @current-change="getData" />
                </div>
            </el-aside>
            <el-main height="">

            </el-main>
        </el-container>

        <template #footer>
            <span>
                <el-button @click=" dialogVisible = false">取消</el-button>
                <el-button type="primary" @click="">确认</el-button>
            </span>
            
        </template>
    </el-dialog>

</template>
<script setup>
import { ref } from 'vue'
// import {
//     useInitTable
// } from '~/composables/useCommon.js'

// const {
//     limit,
//     getData,
//     currentPage,
//     tableData,
//     total
// } = useInitTable()

const dialogVisible = ref(false)

const open = () => dialogVisible.value = true

defineExpose({
    open
})
</script>

<style>

</style>
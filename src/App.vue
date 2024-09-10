<script setup>
import Edit from './components/Edit.vue'
import axios from 'axios'
import {ref, onMounted} from 'vue'

// TODO: 列表渲染
// 思路:声明响应式list->调用接口获取数据->后端数据赋值给list->绑定到table组件
const list = ref([])
const getList = async () =>{
  const res = await axios.get('/list')
  list.value = res.data
}
onMounted(() => getList())

// TODO: 删除功能
// 思路：获取当前行id->通过id调用删除接口->调用get接口获取最新列表
const onDelete = async(id)=>{
  await axios.delete(`/del/${id}`)
  getList()
}


// TODO: 编辑功能
// 思路:打开弹框->回填数据>更新数据
// 1.打开弹框(获取子组件实例调用方法或者修改属性)
// 2.回调数据(调用详情接口/当前行的静态数据)

const editRef = ref(null)
const openDialog = (item) => {
  console.log('item', item)
  editRef.value.open(item)
}


//更新列表
const updateList = () =>{
  getList()
}

</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{row}">
          <el-button type="primary" link @click="openDialog(row)">编辑</el-button>
          <el-button type="danger" link @click="onDelete(row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit ref="editRef" @update-list="updateList" />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>

<template>
  <el-table :data="tableData"
            :cell-style="{ textAlign: 'center' }"
            :header-cell-style="{ 'text-align': 'center' }">
    <el-table-column prop="id" label="ID" width="80"></el-table-column>
    <el-table-column prop="name" label="Name" width="180"></el-table-column>
    <el-table-column prop="usage" label="Usage" width="100"></el-table-column>
    <el-table-column prop="work" label="Work" width="180"></el-table-column>

    <!-- 新增的按钮列 -->
    <el-table-column label="操作" width="150">
      <template #default="{ row }">
        <el-button size="small" @click="handleButtonClick(row)">操作</el-button>
      </template>
    </el-table-column>
  </el-table>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import axios from 'axios';
import { ElTable, ElTableColumn, ElButton } from 'element-plus';

const tableData = ref([]);

const fetchData = async () => {
  try {
    const response = await axios.get('http://localhost:3000/pcs');
    tableData.value = response.data;
  } catch (error) {
    console.error('Error fetching data:', error);
    // 处理错误情况，例如显示错误消息
  }
};

let intervalId;

onMounted(() => {
  fetchData(); // 首次加载时获取数据
  intervalId = setInterval(fetchData, 1000); // 每秒刷新一次数据
});

onUnmounted(() => {
  clearInterval(intervalId); // 组件卸载时清除定时器
});

const handleButtonClick = (row) => {
  //弹窗
  alert(JSON.stringify(row, null, 2));
  console.log('Button clicked for row:', row);
  // 处理按钮点击事件
};
</script>

<style>
/* 添加样式 */
</style>

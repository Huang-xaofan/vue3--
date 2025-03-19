<template>
  <el-card shadow="hover" class="box-card">
    <span class="tab">接口调用监测</span>
    <el-table
      :header-cell-style="{ color: '#333333' }"
      :header-align="center"
      :data="tableData"
      style="width: 100%"
      :row-class-name="tableRowClassName"
      :cell-style="cellStyle"
      :row-style="red"
    >
      <el-table-column
        align="center"
        prop="date"
        label="接口名称"
        width="280px"
      >
        <template #default="scope">
          <span
            :style="{
              color: '#000000',
            }"
          >
            {{ scope.row.date }}
          </span>
        </template>
      </el-table-column>
      <el-table-column align="center" prop="name" label="调用时间">
        <template #default="scope">
          <span
            :style="{
              color: '#000000',
            }"
          >
            {{ scope.row.name }}
          </span>
        </template>
      </el-table-column>
      <el-table-column align="center" prop="address" label="调用状态">
        <template #default="scope">
          <span
            :style="{
              color: scope.row.address === '失败' ? '#c80a0a' : '#000000',
            }"
          >
            {{ scope.row.address }}
          </span>
        </template>
      </el-table-column>
      <el-table-column align="center" prop="NumberCalls" label="调用次数(次)">
        <template #default="scope">
          <span
            :style="{
              color: '#000000',
            }"
          >
            {{ scope.row.NumberCalls }}
          </span>
        </template>
      </el-table-column>
      <el-table-column
        align="center"
        prop="CallDuration"
        label="平均调用时长(毫秒)"
      >
        <template #default="scope">
          <span
            :style="{
              color: '#000000',
            }"
          >
            {{ scope.row.CallDuration }}
          </span>
        </template>
      </el-table-column>
      <el-table-column
        align="center"
        prop="viewl"
        label="
查看"
      >
        <template #default="scope">
          <span
            :style="{
              color: '#000000',
            }"
          >
            {{ scope.row.viewl }}
          </span>
        </template>
      </el-table-column>
    </el-table>
  </el-card>
</template>

<script setup>
const tableRowClassName = ({ row, rowIndex }) => {
  if (rowIndex % 2 == 1) {
    return "warning-row";
  }
};

const tableData = [
  {
    date: "航空器作业信息",
    name: "2025-01-02  12:31",
    address: "成功",
    NumberCalls: "175",
    CallDuration: "10",
    viewl: "166",
  },
  {
    date: "航空器作业信息",
    name: "2025-01-02  12:31",
    address: "成功",
    NumberCalls: "170",
    CallDuration: "10",
    viewl: "343",
  },
  {
    date: "航空器作业信息",
    name: "2025-01-02  12:31",
    address: "成功",
    NumberCalls: "465",
    CallDuration: "10",
    viewl: "912",
  },
  {
    date: "航空器作业信息",
    name: "2025-01-02  12:31",
    address: "失败",
    NumberCalls: "308",
    CallDuration: "10",
    viewl: "690",
  },
  {
    date: "航空器作业信息",
    name: "2025-01-02  12:31",
    address: "成功",
    NumberCalls: "490",
    CallDuration: "10",
    viewl: "286",
  },
  {
    date: "航空器作业信息",
    name: "2025-01-02  12:31",
    address: "成功",
    NumberCalls: "907",
    CallDuration: "10",
    viewl: "624",
  },
];
</script>

<style scoped>
::v-deep(.warning-row) {
  background: #e7f0f0;
}
.el-table .warning-row {
  background: #e7f0f0;
}
.tab {
  font-weight: 700;
  color: #333333;
  font-size: 20px;
  margin-left: -10px;
  position: relative;
  top: -10px;
}
</style>

<template>
  <el-table :data="fileTableData">
    <el-table-column prop="fileName" label="文件名" />
    <el-table-column prop="modifiedDateTime" label="修改时间" />
    <el-table-column prop="fileSize" label="文件大小" />
  </el-table>
</template>

<script>
export default {
  data() {
    return {
      fileTableData: [{
        fileName: "1",
        modifiedDateTime: "222",
        fileSize: 231,
      }],
    };
  },
  method: {
    
  },
};
</script>

<style></style>

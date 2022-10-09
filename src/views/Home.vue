<template>
  <el-button @click="refreshFileTableData()">刷新</el-button>

  <el-table :data="fileTableData">
    <el-table-column prop="fileName" label="文件名" min-width="80%" />
    <el-table-column prop="lastModifiedTime" label="上次修改时间" min-width="20%" />
    <el-table-column prop="fileSize" label="文件大小" min-width="20%" />
  </el-table>
</template>

<script>
import axios from "axios"

export default {
  data() {
    return {
      fileTableData: [
        {
          fileName: "1",
          modifiedDateTime: "222",
          fileSize: 231,
        },
      ],
    }
  },
  methods: {
    refreshFileTableData() {
      axios
        .get("/api/file/", {
          params: {
            path: "D:/Programming/Project/ss-cloud-server/src/main/resources/static",
          },
        })
        .then((res) => {
          this.fileTableData = res.data.data
        })
    },
  },
}
</script>

<style></style>

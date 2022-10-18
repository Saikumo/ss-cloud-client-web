<template>
  <el-button @click="refreshFileTable()">刷新</el-button>
  <el-button @click="back()">返回上一级目录</el-button>

  <el-table :data="fileTableData" @cell-dblclick="handleCellDoubleClick">
    <el-table-column type="selection" width="55" />
    <el-table-column prop="fileName" label="文件名" min-width="80%" />
    <el-table-column
      prop="lastModifiedTime"
      label="上次修改时间"
      min-width="20%"
    />
    <el-table-column prop="fileSize" label="文件大小" min-width="20%" />
  </el-table>
</template>

<script>
import axios from "axios"

export default {
  data() {
    return {
      fileTableData: [],
      filePathArr: [],
    }
  },
  methods: {
    refreshFileTable() {
      axios
        .get("/api/file/", {
          params: {
            filePath: "/" + this.filePathArr.join("/"),
          },
        })
        .then((res) => {
          this.fileTableData = res.data.data
        })
    },
    handleCellDoubleClick(row, column, cell, event) {
      // refresh fileTable if it is a directory
      if (row.isDirectory) {
        this.filePathArr = row.filePath.split("/")
        this.refreshFileTable()
      }
    },
    back() {
      this.filePathArr.pop()
      this.refreshFileTable()
    },
  },
  mounted() {
    this.refreshFileTable()
  },
}
</script>

<style></style>

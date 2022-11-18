<template>
  <el-row :gutter="20">
    <el-button style="margin-left: 10px;" @click="refreshFileTable()">刷新</el-button>
    <el-button @click="back()">返回上一级目录</el-button>
    <el-upload style="margin-left: 10px" :show-file-list="false" :http-request="handleUpload" multiple>
      <el-button>上传</el-button>
    </el-upload>
    <el-button style="margin-left: 10px;" @click="handleDownload" @selection-change="handleSelectionChange">下载
    </el-button>
  </el-row>

  <el-table :data="fileTableData" @cell-dblclick="handleCellDoubleClick" stripe>
    <el-table-column type="selection" width="55" />
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
    handleUpload(options) {
      let formData = new FormData()
      formData.append("multipartFile", options.file)

      axios
        .post("/api/file/upload", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
          params: {
            filePath: "/" + this.filePathArr.join("/"),
            overwriteFlag: false,
          },
        })
        .then((res) => {
          console.log(res)
          if (res.data.statusCode === 20000) {
            this.refreshFileTable()
            this.$message.success("上传成功")
          } else {
            this.$message.error(res.data.statusMessage)
          }
        })
    },
    handleSelectionChange(){

    },
    handleDownload(){
      axios.get("/api/file/download",{
        params:{
          filePath: "/test.txt"
        }
      }).then(res => {
        console.log(res)
      })
    },
  },
  mounted() {
    this.refreshFileTable()
  },
}
</script>

<style>

</style>

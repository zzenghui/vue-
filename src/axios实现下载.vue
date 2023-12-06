<script setup lang='ts' >
//使用axios实现下载功能
import axios from 'axios';
const handleDownload = () => {
    axios.get("http://localhost:5173/excel.xlsx", { responseType: "arraybuffer" }).then((res) => {
        const blod = new Blob([res.data])
        const blodUrl = window.URL.createObjectURL(blod)
        const tmpLink = document.createElement("a")
        tmpLink.style.display = "none"
        tmpLink.href = blodUrl
        const fileName = "excel.xlsx"   //文件的名称
        tmpLink.setAttribute("download", fileName)
        document.body.appendChild(tmpLink)
        tmpLink.click()
        document.body.removeChild(tmpLink)
        window.URL.revokeObjectURL(blodUrl)
    })
}
</script>

<template>
    <el-button @click="handleDownload">下载</el-button>
</template>
 
<style scoped></style>
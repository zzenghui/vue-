<script setup lang='ts' >

// axios实现大文件的分片上传
import axios from "axios"
// import md5 from "md5"

//省略进度条写法

async function handleUpload(e: Event) {
  let chunkSize = 10 * 1024 * 1024
  const target = e.target as HTMLInputElement
  if (target.files) {
    const file = target.files[0]
    const { name, size } = target.files[0]
    let start = 0 //当前的偏移量
    let index = 0 //当前切了多少片
    while (start < size) {
      let blod = null
      //这里需要判断最后一段上传时的分片大小是否超出文件大小
      if (start + chunkSize > size) {
        blod = file.slice(start, size)
      } else {
        blod = file.slice(start, start + chunkSize)
      }
      start += chunkSize
      let boldFile = new File([blod], name)
      let formData = new FormData()
      formData.append("file", boldFile)
      formData.append("index", index + "")
      await axios.post("/api/upload", formData)
      index++
    }
    //通知后端进行文件合并
    axios.get("合并文件的路径").then(() => {
      alert("上传完成")
    })
  }

}
</script>

<template>
  <input type="file" @change="handleUpload">
</template>
 
<style scoped></style>
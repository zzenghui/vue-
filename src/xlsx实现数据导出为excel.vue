<script setup lang='ts' >
//Vue3 Element Plus表格导出（带图片导出）、导入功能
// npm install js-table2excel  实现将表格内容转成excel
// npm install xlsx
// import table2Excel from 'js-table2excel' // 引入  js-table2excel
import * as XLSX from "xlsx" // 引入  xlsx
const tableData = [
    {
        date: '2016-05-03',
        name: 'Tom',
        address: 'No. 189, Grove St, Los Angeles',
    },
    {
        date: '2016-05-02',
        name: 'Tom',
        address: 'No. 189, Grove St, Los Angeles',
    },
    {
        date: '2016-05-04',
        name: 'Tom',
        address: 'No. 189, Grove St, Los Angeles',
    },
    {
        date: '2016-05-01',
        name: 'Tom',
        address: 'No. 189, Grove St, Los Angeles',
    },
]

/**
 * @description: 导出excel
 * @param {any} dataList   导出的数据
 * @param {Array} fields   需要导出的字段
 * @param {Array} headers  导出的头部
 * @param {string} fileName 需要加后缀名 eg: 'test.xlsx'
 * @param {string} sheetName
 * @return {*}
 */
function exportExcel(dataList: any, fields: Array<string>, headers: Array<string> = [], fileName: string = 'Excel.xlsx', sheetName: string = 'Sheet') {
    let data = new Array()
    if (!Array.isArray(dataList)) return console.warn('dataList is not an array type')
    if (!Array.isArray(fields)) return console.warn('fields is not an array type')
    if (!Array.isArray(headers)) return console.warn('headers is not an array type')

    data = dataList.map((obj) => {
        return fields.map((field) => {
            return obj[field]
        })
    })
    if (headers.length > 0) {
        data.splice(0, 0, headers)
    } else {
        // 将headers设置为英文字段表头
        data.splice(0, 0, fields)
    }
    const ws = XLSX.utils.aoa_to_sheet(data) // 创建工作表
    const wb = XLSX.utils.book_new() // 创建工作簿

    // 隐藏表头
    // let wsrows = [{ hidden: true }]
    // ws['!rows'] = wsrows // ws - worksheet

    XLSX.utils.book_append_sheet(wb, ws, sheetName) // 将工作表添加到工作簿中
    XLSX.writeFile(wb, fileName) // 导出文件
}

const toExportExcel = () => {
    exportExcel(tableData, ["date", "name", "address"], ["日期", "名称", "地址"])
}
</script>

<template>
    <el-button type="primary">导入文件</el-button>
    <el-button type="primary" @click="toExportExcel">导出文件</el-button>
    <el-table :data="tableData" style="width: 100%">
        <el-table-column prop="date" label="Date" width="180" />
        <el-table-column prop="name" label="Name" width="180" />
        <el-table-column prop="address" label="Address" />
    </el-table>
</template>
 
<style scoped></style>
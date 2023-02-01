<script setup>
    import {ref, watchEffect} from "vue"
    import ExcelJs from 'exceljs'

    `
    npm install exceljs
    傳入的資料格式
    data = [
                {
                    colName11: val11,
                    colName12: val12,
                    colName13: val13,...
                },
                {
                    colName21: val21,
                    colName22: val22,
                    colName23: val23,...
                }
            ]
    `

    const PropDatas = defineProps(['data']);
    const data = ref([]);
    const columns = ref([]);
    watchEffect(() => {
        if (PropDatas.data.length >0) {
            Object.keys(PropDatas.data[0]).forEach((col) => {
            columns.value.push({name: col});
            })

            Object.keys(PropDatas.data).forEach((i) => {
                data.value.push(Object.values(PropDatas.data[i]))
            })
        }
    })



    function download() {
        const workbook = new ExcelJs.Workbook();           //cerate workbook
        const sheet = workbook.addWorksheet('工作表1');     //add sheet
        // data.value.push([''])                              //when row is empty
        sheet.addTable({
                name: 'Table',
                ref: 'A1',                                    //start location
                columns: columns.value,                       //set columns name
                rows: data.value                              //set rows' data
            })

        workbook.xlsx.writeBuffer()
        .then((content) => {                                //download Excel
        const link = document.createElement("a");
        const blobData = new Blob([content], {
            type: "application/vnd.ms-excel;charset=utf-8;"
        });
        link.download = 'output.xlsx';
        link.href = URL.createObjectURL(blobData);
        link.click();
        });
    }

</script>

<template>
    <el-button type="primary" @click="download">Export Excel</el-button>
</template>
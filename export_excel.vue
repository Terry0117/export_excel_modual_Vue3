<script setup>
    import {ref, watchEffect} from "vue"
    import ExcelJs from 'exceljs'
    import excel from './icons/Excel.vue'

    `
    npm install exceljs
    傳入的資料格式
    data = [
                {
                    colName1: val11,
                    colName2: val12,
                    colName3: val13,...
                },
                {
                    colName1: val21,
                    colName2: val22,
                    colName3: val23,...
                }
            ]
    `

    const PropDatas = defineProps(['data','file_name']);
    const data = ref([]);
    const columns = ref([]);
    const file_name = PropDatas.file_name + '.xlsx';
    watchEffect(() => {
        data.value = [];
        columns.value = [];
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
        link.download = file_name;
        link.href = URL.createObjectURL(blobData);
        link.click();
        });
    }

</script>

<template>
    <el-button @click="download" text><excel/></el-button>
</template>
# export_excel_modual_Vue3

## Install Extension

```sh
npm install exceljs
```

## Data Format
```sh
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
```
## Use In VUE3

```sh
<script setup>
    import {ref, onBeforeMount} from 'vue'
    import exportExcel from 'your folder/export_excel.vue'

    const json_data = ref([]);
    
    onBeforeMount(() => {
        json_data.value = `your data`;
    })
</script>

<template>
    <exportExcel :data="json_data" :file_name="your file name" />
</template>
```

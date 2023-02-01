# export_excel_modual_Vue3

## Install Extension

```sh
npm install exceljs
```

## Data Format
```sh
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
    <exportExcel :data="json_data" />
</template>
```

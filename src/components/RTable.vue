<template>
    <el-table
            :data="tableData"
            border
            @selection-change="handleSelectionChange"
            :cell-style="{height:'38px',padding:'0'}"
            :header-cell-style="{background:'#f2f2f2',height:'38px',padding:'0'}"
            style="width: 100%">
        <el-table-column
                v-if="isSelection"
                type="selection"
                width="55">
        </el-table-column>

        <el-table-column
                v-for="(item,index) in tableCols"
                :prop="item.prop"
                :label="item.label"
                :width="item.width"
                :key="index"
                show-overflow-tooltip>
            <template slot-scope="scope">
                <slot v-if="item.slot" :name="item.slot" :data="scope.row"></slot>
                <slot v-else>{{scope.row[item.prop]}}</slot>
            </template>
        </el-table-column>
    </el-table>
</template>

<script>
    export default {
        name: "RTable",
        props: {
            tableData: {
                type: Array
            },
            tableCols: {
                type: Array
            },
            isSelection: {
                type: Boolean,
                default: true
            }
        },
        data() {
            return {
                selection: []
            }
        },
        methods: {
            handleSelectionChange(selection) {
                this.selection = selection;
            }
        }
    }
</script>

<style scoped>

</style>
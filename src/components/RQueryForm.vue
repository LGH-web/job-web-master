<template>
    <el-form :inline="true" :model="form" style="height: 60px">
        <template v-for="(item,index) in items">
            <template v-if="['text','number'].indexOf(item.type) != -1">
                <el-form-item :label="item.label" :key="index">
                    <el-input v-model="form[item.name]" :placeholder="item.placeholder"></el-input>
                </el-form-item>
            </template>
            <template v-if="['select'].indexOf(item.type) != -1">
                select
            </template>
        </template>
        <el-form-item>
            <el-button @click="$emit('search')" icon="el-icon-search" type="primary">查询</el-button>
        </el-form-item>
    </el-form>
</template>

<script>
    export default {
        name: "RQueryForm",
        props: {
            items: {
                type: Array,
                default: () => {
                }
            },
            form: {
                type: Object,
                default: () => {
                }
            }
        }
    }
</script>

<style scoped>

</style>

<template>
    <div>
        <r-query-form :form="form" :items="items" @search="search"></r-query-form>
        <div class="btn-panel">
            <el-button size="small" type="danger" icon="el-icon-delete" @click="del">删除</el-button>
            <el-button size="small" type="primary" icon="el-icon-plus" @click="create" >新增</el-button>
            <el-button size="small" type="primary" icon="el-icon-edit" @click="update">修改</el-button>
        </div>

        <r-table ref="mutipleTable" :tableData="tableData" :tableCols="tableCols">
            <template slot="sex_slot" slot-scope="scope">
                <span v-if="scope.data.sex==0">女</span>
                <span v-else>男</span>
            </template>
        </r-table>
        <r-pagination :page="page" :total="total" @handleCurrentChange="handleCurrentChange"></r-pagination>
        <!--新增组件-->
        <Add v-if="add.visible" :param="add"></Add>
        <!--编辑-->
        <Edit v-if="edit.visible" :param="edit"></Edit>
    </div>
</template>

<script>
    import {query, del} from "@/api/student";
    import RTable from "../../components/RTable";
    import RPagination from "../../components/RPagination";
    import RQueryForm from "../../components/RQueryForm";
    import Edit from "./Edit";
    import Add from "./Add";


    export default {
        name: "Student",
        components: {RQueryForm, RPagination,Add,Edit, RTable},
        data() {
            return {
                tableData: [],
                tableCols: [
                    {prop: 'id', label: 'ID', width: 80},
                    {prop: 'name', label: '姓名'},
                    {prop: 'birthday', label: '出生日期'},
                    {prop: 'professionId', label: '专业'},
                    {prop: 'college', label: '学院'},
                    {prop: 'education', label: '学历'},
                    {prop: 'phone', label: '手机号'},
                    {prop: 'sex', label: '性别', slot: 'sex_slot'},
                    {prop: 'graduateDate', label: '毕业时间'},
                ],
                total: 0,
                page: 1,
                form: {
                    name: ''
                },
                //显示添加页面
                add:{
                    visible:false,
                    close:this.close,
                    callback:this.search
                },
                //显示修改页面
                edit:{
                    visible:false,//控件隐藏
                    close:this.close,//关闭控件
                    callback:this.search,//回调函数,关闭后再次刷新列表
                    form:null,
                },
                items: [
                    {type: 'text', label: '姓名', name: 'name', placeholder: '按姓名查询'},
                    {type: 'text', label: '学院', name:'college',placeholder:'按关键字查询'},
                ],
            }
        },
        mounted() {
            this.list({});
        },
        methods: {
            handleCurrentChange(val) {
                this.page = val;
                let params = this.form;
                params.page = this.page;
                this.list(params);
            },
            search() {
                this.list(this.form);
            },
            list(params) {
                query(params).then(res => {
                    this.tableData = res.data;
                    this.total = res.total;
                })
            },
            //关闭方法
            close(){
                this.add.visible=false;
                this.edit.visible=false;
            },
            update(){
                //获取被选中的条数,this.$refs['mutipleTable']表示引用id为mutipleTable的组件
                let selection = this.$refs['mutipleTable'].selection;
                if(selection.length==1){
                    this.edit.visible = true;
                    this.edit.form = selection[0]
                }else{
                    this.$message.warning('请选择一条数据修改')
                }
            },
            create(){
                this.add.visible = true;
            },
            del() {
                let selections = this.$refs['mutipleTable'].selection;
                if (selections.length > 0) {
                    this.$confirm("确定有删除吗？", "删除提示").then(() => {
                        let arr = selections.map(item => item.id);
                        let ids = arr.join(',');
                        del(ids).then(res => {
                            this.$message.success(res.msg);
                            this.search();
                        })
                    }).catch(() => {
                    })
                } else {
                    this.$message.warning("请选择要删除的数据");
                }
            }
        }
    }
</script>

<style scoped>

</style>

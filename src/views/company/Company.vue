<!--单位管理-管理员-->
<template>
    <div>
        <r-query-form :form="form" :items="items" @search="search"></r-query-form>
        <div class="btn-panel">
          <el-button size="small" type="primary" icon="el-icon-plus" @click="create" >新增</el-button>
          <el-button size="small" type="primary" icon="el-icon-edit" @click="update">修改</el-button>
          <el-button size="small" type="danger" icon="el-icon-delete" @click="del">删除</el-button>
        </div>
        <r-table ref="mutipleTable" :tableData="tableData" :tableCols="tableCols">
          <template slot="slot_status" slot-scope="scope">
            <el-tag v-if="scope.data.status==1" type="success" size="mini" >审核通过</el-tag>
            <el-tag v-else type="danger" size="mini" >审核不通过</el-tag>
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
    import {query,del} from "@/api/company";
    import RTable from "@/components/RTable";
    import RPagination from "@/components/RPagination";
    import {message} from "@/utils/message";
    import RQueryForm from "@/components/RQueryForm";
    import Edit from "./Edit";
    import Add from "./Add";
    export default {
        name: "Company",
        components:{RQueryForm, RPagination,Edit,Add,RTable},
        data(){
            return{
                total:0,
                page:1,
                tableData:[],
                tableCols:[
                    {prop:'id', label:'ID', width:80},
                    {prop:'name', label:'名称'},
                     {prop:'account', label: '账号名称'},
                    {prop:'password', label: '账号密码'},
                    {prop:'type', label:'类型'},
                    {prop:'contact', label:'联系人'},
                    {prop:'telephone', label:'联系电话'},
                    {prop:'email', label:'邮箱'},
                    {prop:'addr', label:'地址'},
                    {prop:'size', label:'规模'},
                    {prop:'status', label:'状态',slot:'slot_status'},
                ],
                form:{
                    name:'',
                    contact:''
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
                items:[
                    {type:'text',label:'名称',name:'name',placeholder:'按关键字查询'},
                    {type:'text',label:'联系人',name:'contact',placeholder:'按关键字查询'},
                ],
            }
        },
        mounted() {
            this.list({})
        },
        methods:{
            handleCurrentChange(val){
                this.page = val;
                let params = this.form;
                params.page = this.page
                this.list(params);
            },
            search(){
                this.list(this.form);
            },
            list(params){
                query(params).then(res=>{
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
              message.warning('请选择一条数据修改')
            }
          },
          create(){
              this.add.visible = true;
          },
            del(){
                let selection = this.$refs['mutipleTable'].selection;
                if(selection.length>0){
                    this.$confirm('确定要删除吗','删除提示').then(()=>{
                        let arr = selection.map(item=>item.id);
                        let ids = arr.join(',');
                        del(ids).then(res=>{
                            message.success(res.msg);
                            this.search()
                        })
                    }).catch(()=>{})
                }else{
                    message.warning('请选择要删除的数据')
                }
            }
        }
    }
</script>

<style scoped>

</style>

<template>
  <el-dialog title="修改" :visible.sync="param.visible" @close="param.close" :close-on-click-modal="false">
    <r-form ref="myForm" :rules="rules" :form="form" :items="items" :save="save"></r-form>
  </el-dialog>
</template>

<script>
import {update} from "@/api/student";
import RForm from "@/components/RForm";
export default {
  name: "Edit",
  props:{
    param:{
      type:Object,
      default:()=>{}
    }
  },
  components:{RForm},
  data(){
    return{
      form:{
        id:'',
        name:'',
        sex:'',
        account:'',
        password:'',
        birthday:'',
        professionId:'',
        college:'',
        education:'',
        phone:'',
        graduateDate:'',
      },
      rules:{
        name:[{required:true,message:'姓名不能为空'}],
        sex:[{required:true,message:'性别不能为空'}],
        account:[{required:true,message:'账号名称不能为空'}],
        password:[{required:true,message:'密码不能为空'}],
        birthday:[{required:true,message:'出生日期不能为空'}],
        professionId:[{required:true,message:'学生专业不能为空'}],
        college:[{required:true,message:'学院不能为空'}],
        education:[{required:true,message:'学历不能为空'}],
        phone:[{required:true,message:'电话号码不能为空'}],
        graduateDate:[{required:true,message:'毕业时间不能为空'}],
      },
      items:[
        {type:'text',label:'学生姓名',prop:'name',name:'name',placeholder:'请录入学生姓名'},
        {
          type:'select',label:'性别',prop:'sex',name:'sex',placeholder:'请选择学生性别',
          options:[{value:0,label:'男'},{value:1,label:'女'}]
        },
        {type:'text',label:'账号名称',prop:'account',name:'account',placeholder:'请录入账号名称'},
        {type:'text',label:'密码',prop:'password',name:'password',placeholder:'请录入账号密码'},
        {type:'date',label:'出生日期',prop:'birthday',name:'birthday',placeholder:'请录入出生日期'},
        {type:'text',label:'专业',prop:'professionId',name:'professionId',placeholder:'请录入学生专业'},
        {type:'text',label:'学院',prop:'college',name:'college',placeholder:'请录入学生学院'},
        {
          type:'select',label:'学历',prop:'education',name:'education',placeholder:'请选择学历',
          options:[
            {value:'高中及以下',label:'高中及以下'},
            {value:'专科',label:'专科'},
            {value:'本科',label:'本科'},
            {value:'硕士研究生',label:'硕士研究生'},
            {value:'博士研究生',label:'博士研究生'},
            {value:'博士后',label:'博士后'},
          ]
        },
        {type:'text',label:'电话号码',prop:'phone',name:'phone',placeholder:'请录入学生电话号码'},
        {type:'date',label:'毕业时间',prop:'graduateDate',name:'graduateDate',placeholder:'请录入学生毕业时间'},
      ]
    }
  },
  mounted() {
    this.form = JSON.parse(JSON.stringify(this.param.form));
  },
  methods:{
    save(){
      let flag = this.$refs['myForm'].validateForm();
      console.log("这个值是"+this.form.status);
      if(flag){
        update(this.form).then(res=>{
          this.$message.success(res.msg);
          this.param.close();
          this.param.callback();
        })
      }
    }
  }
}
</script>

<style scoped>

</style>

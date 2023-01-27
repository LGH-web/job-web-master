<template>
  <el-dialog title="修改" :visible.sync="param.visible" @close="param.close" :close-on-click-modal="false">
    <r-form ref="myForm" :rules="rules" :form="form" :items="items" :save="save"></r-form>
  </el-dialog>
</template>

<script>
import {update} from "@/api/company";
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
        account:'',
        password:'',
        type:'',
        contact:'',
        telephone:'',
        email:'',
        addr: '',
        size:'',
        status:0,
      },
      rules:{
        name:[{required:true,message:'姓名不能为空'}],
        account:[{required:true,message:'账号名称不能为空'}],
        password:[{required:true,message:'账号密码不能为空'}],
        type:[{required:true,message:'单位类型不能为空'}],
        contact:[{required:true,message:'单位联系人不能为空'}],
        telephone:[{required:true,message:'单位电话不能为空'}],
        email:[{required:true,message:'单位邮箱不能为空'}],
        addr:[{required:true,message:'单位地址不能为空'}],
        size:[{required:true,message:'单位规模不能为空'}],
      },
      items:[
        {type:'text',label:'单位名称',prop:'name',name:'name',placeholder:'请录入单位名称'},
        {type:'text',label:'账号名称',prop:'account',name:'account',placeholder:'请录入账号名称'},
        {type:'text',label:'账号密码',prop:'password',name:'password',placeholder:'请录入账号密码'},
        {type:'text',label:'单位类型',prop:'type',name:'type',placeholder:'请录入单位类型'},
        {type:'text',label:'单位联系人',prop:'contact',name:'contact',placeholder:'请录入单位联系人'},
        {type:'text',label:'单位电话',prop:'telephone',name:'telephone',placeholder:'请录入单位电话'},
        {type:'text',label:'单位邮箱',prop:'email',name:'email',placeholder:'请录入单位邮箱'},
        {type:'text',label:'单位地址',prop:'addr',name:'addr',placeholder:'请录入单位地址'},
        {type:'text',label:'单位规模',prop:'size',name:'size',placeholder:'请录入单位规模'},
        {
          type:'select',label:'审核状态',prop:'status',name:'status',placeholder:'请选择审核状态',
          options:[{value:0,label:'不通过'},{value:1,label:'通过'}]
        },
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

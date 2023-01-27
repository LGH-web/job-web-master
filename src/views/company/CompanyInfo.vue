<template>
    <div>
        <r-form ref="myForm" :rules="rules" :form="form" :items="items" :save="save"></r-form>
    </div>
</template>

<script>
    import {update} from "@/api/company";
    import RForm from "@/components/RForm";
    import {companyInfo} from "@/api/company";
    import {dict_query} from "@/api/dict";
    export default {
        name: "CompanyInfo",
        components: {RForm},
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
                  logo:'',
                  description:'',
                },
                rules:{
                  name:[{required:true,message:'姓名不能为空'}],
                  account:[{required:true,message:'账号名称不能为空'}],
                  password:[{required:true,message:'账号密码不能为空'}],
                  type:[{required:true,message:'单位类型不能为空'}],
                  contact:[{required:true,message:'单位联系人不能为空'}],
                  telephone:[{required:true,message:'单位联系电话号码不能为空'}],
                  email:[{required:true,message:'单位邮箱不能为空'}],
                  addr:[{required:true,message:'单位地址不能为空'}],
                  size:[{required:true,message:'单位规模不能为空'}],
                },
                items:[
                    {type:'text',label:'名称',prop:'name',name:'name',placeholder:'请输入名称'},
                    {type:'text',label:'联系人',prop:'contact',name:'contact',placeholder:'联系人', options:[]},
                    {type:'text',label:'联系电话',prop:'telephone',name:'telephone',placeholder:'联系电话'},
                    {type:'text',label:'邮箱',prop:'email',name:'email',placeholder:'邮箱'},
                    {type:'text',label:'地址',prop:'addr',name:'addr',placeholder:'地址'},
                    {type:'text',label:'外链',prop:'url',name:'url',placeholder:'外链'},
                    {type:'text',label:'网址',prop:'url',name:'url',placeholder:'网址'},
                    {type:'select',label:'规模',prop:'size',name:'size',placeholder:'规模',options:[]},
                    {type:'select',label:'公司类型',prop:'type',name:'type',placeholder:'公司类型',options:[]},
                    {type:'file',label:'Logo',prop:'logo',name:'logo',placeholder:'Logo'},
                    {type:'area',label:'公司描述',prop:'description',name:'description',placeholder:'内容...'},
                ]
            }
        },
        mounted() {
            this.getInfo();
            this.getDictList1(4);
            this.getDictList2(5);
        },
        methods:{
            getDictList1(typeId){
                dict_query({typeId:typeId}).then(res=>{
                    let arr = [];
                    res.data.forEach(item=>{
                        let p = {value:item.dictKey,label:item.dictVal};
                        arr.push(p);
                    });
                    this.items.forEach(item=>{
                        if(item.type=='select' && item.name == 'type'){
                            item.options= arr;
                        }
                    })
                })
            },
            getDictList2(typeId){
                dict_query({typeId:typeId}).then(res=>{
                    let arr = [];
                    res.data.forEach(item=>{
                        let p = {value:item.dictKey,label:item.dictVal};
                        arr.push(p);
                    });
                    this.items.forEach(item=>{
                        if(item.type=='select' && item.name == 'size'){
                            item.options= arr;
                        }
                    })
                })
            },

            getInfo(){
                companyInfo().then(res=>{
                    this.form = res.data;
                    console.log(this.form)
                })
            },
            save(){
                let flag = this.$refs['myForm'].validateForm();
                if(flag){
                    let param = this.form;
                    if(this.form.single){
                        param.single ='Y';
                    }else{
                        param.single ='N';
                    }
                    update(param).then(res=>{
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
